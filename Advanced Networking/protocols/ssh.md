# SSH (Secure Shell)

## Overview

SSH is a secure protocol used for remote login and administration of systems.

Default Port:

```text
22
```

SSH encrypts communication between client and server.

---

## Why SSH Matters

SSH is one of the most important protocols in:
- Linux administration
- DevOps
- Cloud
- Kubernetes
- SRE

Used for:
- remote access
- automation
- Git operations
- file transfer
- infrastructure management

---

## SSH Architecture

Client
→ SSH Server (sshd)

---

## SSH Authentication Methods

### Password Authentication

User enters password.

---

### Key-based Authentication

Uses:
- private key
- public key

Much more secure.

---

## SSH Key Generation

### Generate SSH Key

```bash
ssh-keygen -t rsa -b 4096
```

Files created:

```text
~/.ssh/id_rsa
~/.ssh/id_rsa.pub
```

---

## SSH Connection

### Connect to Server

```bash
ssh user@server-ip
```

Example:

```bash
ssh ubuntu@192.168.1.10
```

---

## Copy Public Key

```bash
ssh-copy-id user@server-ip
```

---

## SSH Configuration File

Location:

```text
~/.ssh/config
```

Example:

```text
Host webserver
    HostName 192.168.1.10
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
```

Connect easily:

```bash
ssh webserver
```

---

## SCP (Secure Copy)

### Copy File to Remote Server

```bash
scp file.txt user@server:/tmp
```

### Copy From Remote

```bash
scp user@server:/tmp/file.txt .
```

---

## SSH Port Forwarding

### Local Port Forwarding

```bash
ssh -L 8080:localhost:80 user@server
```

Useful for:
- databases
- Kubernetes dashboards
- internal services

---

## SSH Agent

### Start Agent

```bash
eval "$(ssh-agent -s)"
```

### Add Key

```bash
ssh-add ~/.ssh/id_rsa
```

---

## Real-world DevOps Usage

SSH used for:
- server administration
- CI/CD pipelines
- Ansible automation
- GitHub authentication
- Kubernetes node access

---

## Kubernetes Relevance

SSH commonly used for:
- node debugging
- kubeadm cluster setup
- troubleshooting worker nodes

---

## Security Best Practices

### Disable Root Login

Edit:

```text
/etc/ssh/sshd_config
```

```text
PermitRootLogin no
```

---

### Disable Password Authentication

```text
PasswordAuthentication no
```

---

### Use SSH Keys

More secure than passwords.

---

### Change Default Port

Reduces automated attacks.

---

## Troubleshooting Scenarios

### Scenario 1: Connection Refused

Check:

```bash
systemctl status sshd
```

Possible causes:
- ssh service stopped
- firewall blocked
- wrong port

---

### Scenario 2: Permission Denied

Possible causes:
- wrong key permissions
- invalid public key
- incorrect username

Fix permissions:

```bash
chmod 600 ~/.ssh/id_rsa
chmod 700 ~/.ssh
```

---

### Scenario 3: SSH Timeout

Possible causes:
- routing issue
- firewall
- security group block

Test:

```bash
ping server-ip
telnet server-ip 22
```

---

## Interview Questions

1. What is SSH?
2. Difference between SSH and Telnet?
3. How does SSH key authentication work?
4. What is SSH tunneling?
5. How to secure SSH?

---

## Summary

SSH is the backbone of Linux administration and secure infrastructure management.