# Automation

Automation reduces manual work, improves consistency, and enables DevOps at scale.

---

# Automation Levels

```
Level 1: Manual Tasks → Scripts
Level 2: Scripts → Configuration Management (Ansible)
Level 3: Infrastructure + Config → Infrastructure as Code (Terraform)
Level 4: Everything → GitOps (ArgoCD, Flux)
```

---

# Shell Script Automation

Simple but powerful for ad-hoc tasks.

## Example 1: Automated Deployment

```bash
#!/bin/bash
set -euo pipefail

APP_NAME="myapp"
DEPLOY_PATH="/opt/$APP_NAME"
REPO="https://github.com/user/repo.git"
BRANCH="${1:-main}"

echo "Deploying $APP_NAME from $BRANCH..."

# Clone or update repository
if [ -d "$DEPLOY_PATH" ]; then
  cd "$DEPLOY_PATH"
  git pull origin "$BRANCH"
else
  git clone -b "$BRANCH" "$REPO" "$DEPLOY_PATH"
  cd "$DEPLOY_PATH"
fi

# Build and deploy
npm install
npm run build
systemctl restart "$APP_NAME"

echo "✓ Deployment complete"
```

## Example 2: Automated Backup & Cleanup

```bash
#!/bin/bash
set -euo pipefail

BACKUP_DIR="/backups"
RETENTION_DAYS=7

# Create backup
echo "Creating backup..."
tar -czf "$BACKUP_DIR/backup_$(date +%Y%m%d_%H%M%S).tar.gz" /opt/myapp

# Remove old backups
echo "Cleaning up old backups..."
find "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +$RETENTION_DAYS -delete

echo "✓ Backup complete"
```

---

# Ansible Automation

Agentless automation for infrastructure configuration.

## Installation

```bash
# Ubuntu/Debian
sudo apt install ansible -y

# CentOS/RHEL
sudo yum install ansible -y

# Check version
ansible --version
```

## Basic Inventory File

```ini
# /etc/ansible/hosts
[webservers]
web1.example.com ansible_user=ubuntu
web2.example.com ansible_user=ubuntu

[databases]
db1.example.com ansible_user=ubuntu

[all:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

## Example Playbook: Web Server Setup

```yaml
---
- name: Setup Web Servers
  hosts: webservers
  become: yes  # Run with sudo

  vars:
    nginx_port: 80
    app_user: appuser

  tasks:
    # System updates
    - name: Update system packages
      apt:
        update_cache: yes
        upgrade: dist
      when: ansible_os_family == "Debian"

    # Install dependencies
    - name: Install required packages
      apt:
        name:
          - nginx
          - python3-pip
          - git
        state: present

    # Create application user
    - name: Create app user
      user:
        name: "{{ app_user }}"
        shell: /bin/bash
        createhome: yes

    # Clone repository
    - name: Clone application repository
      git:
        repo: https://github.com/user/app.git
        dest: /opt/app
        version: main
        update: yes
      become_user: "{{ app_user }}"

    # Install Python dependencies
    - name: Install Python dependencies
      pip:
        requirements: /opt/app/requirements.txt
        executable: pip3

    # Configure Nginx
    - name: Create Nginx config
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/sites-available/app
      notify: Restart Nginx

    # Start services
    - name: Start and enable Nginx
      systemd:
        name: nginx
        state: started
        enabled: yes

  handlers:
    - name: Restart Nginx
      systemd:
        name: nginx
        state: restarted
```

## Running Ansible Commands

```bash
# Run playbook
ansible-playbook playbook.yml

# Run on specific hosts
ansible-playbook playbook.yml -i hosts --limit webservers

# Check mode (dry-run)
ansible-playbook playbook.yml --check

# With more verbose output
ansible-playbook playbook.yml -vv

# Run ad-hoc command
ansible webservers -m command -a "systemctl restart nginx"

# Check connectivity
ansible all -m ping
```

## Example: Database Backup Playbook

```yaml
---
- name: Database Backup Automation
  hosts: databases
  become: yes

  vars:
    backup_path: /backups/databases
    retention_days: 7
    db_user: postgres

  tasks:
    - name: Create backup directory
      file:
        path: "{{ backup_path }}"
        state: directory
        owner: "{{ db_user }}"
        mode: '0755'

    - name: Create database backup
      shell: |
        pg_dump -U postgres mydatabase | gzip > {{ backup_path }}/db_$(date +\%Y\%m\%d_\%H\%M\%S).sql.gz
      become_user: "{{ db_user }}"

    - name: Remove old backups
      shell: |
        find {{ backup_path }} -name "db_*.sql.gz" -mtime +{{ retention_days }} -delete
```

---

# Terraform Automation

Infrastructure as Code for cloud resources.

## Installation

```bash
# Download from terraform.io
wget https://releases.hashicorp.com/terraform/1.0.0/terraform_1.0.0_linux_amd64.zip
unzip terraform_1.0.0_linux_amd64.zip -d /usr/local/bin
terraform version
```

## Basic Terraform Structure

```bash
main.tf              # Main configuration
variables.tf         # Input variables
outputs.tf           # Output values
terraform.tfvars     # Variable values
```

## Example: AWS EC2 Instance

```hcl
# variables.tf
variable "aws_region" {
  description = "AWS region"
  default     = "us-east-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  default     = "t2.micro"
}

variable "instance_count" {
  description = "Number of instances"
  default     = 2
}

# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

# Create security group
resource "aws_security_group" "web" {
  name = "web-sg"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Create EC2 instances
resource "aws_instance" "web" {
  count           = var.instance_count
  ami             = "ami-0c55b159cbfafe1f0"
  instance_type   = var.instance_type
  security_groups = [aws_security_group.web.name]

  tags = {
    Name = "web-server-${count.index + 1}"
  }
}

# outputs.tf
output "instance_ips" {
  description = "Public IPs of instances"
  value       = aws_instance.web[*].public_ip
}
```

## Terraform Workflow

```bash
# Initialize
terraform init

# Validate configuration
terraform validate

# Format code
terraform fmt

# Plan changes
terraform plan -out=tfplan

# Apply changes
terraform apply tfplan

# Destroy resources
terraform destroy -auto-approve
```

---

# Scheduled Automation with Cron

Run tasks on a schedule.

## Cron Syntax

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 6) (Sunday to Saturday)
│ │ │ │ │
│ │ │ │ │
* * * * * command
```

## Common Examples

```bash
# Every day at 2 AM
0 2 * * * /opt/scripts/backup.sh

# Every Monday at 9 AM
0 9 * * 1 /opt/scripts/weekly-report.sh

# Every 6 hours
0 */6 * * * /opt/scripts/health-check.sh

# Every 30 minutes
*/30 * * * * /opt/scripts/sync.sh

# First day of month at midnight
0 0 1 * * /opt/scripts/monthly-cleanup.sh

# Multiple times per day
0 8,12,18 * * * /opt/scripts/deploy.sh
```

## Setting Up Cron Job

```bash
# Edit crontab
crontab -e

# List cron jobs
crontab -l

# Remove all cron jobs
crontab -r
```

## Cron Best Practices

```bash
#!/bin/bash
# Redirect output to log
0 2 * * * /opt/scripts/backup.sh >> /var/log/backup.log 2>&1

# Use full paths in cron
0 3 * * * /usr/bin/curl https://example.com/health-check

# Send email on failure
0 4 * * * /opt/scripts/maintenance.sh || echo "Maintenance failed" | mail -s "Alert" admin@example.com
```

---

# Practical Automation Workflow

```
1. Identify repetitive tasks
   ↓
2. Create shell script
   ↓
3. Test thoroughly
   ↓
4. Scale with Ansible/Terraform
   ↓
5. Schedule with Cron
   ↓
6. Monitor execution
   ↓
7. Iterate and improve
```

---

# Benefits of Automation

- ✓ **Faster deployments** — minutes instead of hours
- ✓ **Reduced errors** — consistent execution
- ✓ **Better compliance** — reproducible configurations
- ✓ **Scalability** — manage hundreds of servers
- ✓ **Team efficiency** — focus on optimization, not repetition
- ✓ **Cost reduction** — fewer manual hours

---

# Summary

Automation is a core DevOps practice. Start with shell scripts for simple tasks, graduate to Ansible for configuration management, and use Terraform for infrastructure at scale.