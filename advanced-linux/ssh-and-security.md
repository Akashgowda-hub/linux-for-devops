# SSH & Security

SSH (Secure Shell) provides secure remote access to Linux systems through encrypted connections. Proper SSH configuration and firewall management are critical for system security.

---

# SSH Basics

## SSH Login

**Connect to remote server:**
```bash
ssh user@server-ip
ssh user@example.com
```

**Expected output:**
```
The authenticity of host 'example.com (203.0.113.10)' can't be established.
ECDSA key fingerprint is SHA256:abcd1234...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'example.com' (ECDSA) to /home/user/.ssh/known_hosts.
user@example.com's password:
```

**Connect with specific username:**
```bash
ssh -u username 192.168.1.100
```

**Connect on non-standard port:**
```bash
ssh -p 2222 user@example.com
```

---

# SSH Key Management

## Generate SSH Keys

**Generate new key pair (interactive):**
```bash
ssh-keygen
```

**Expected output:**
```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/user/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/user/.ssh/id_rsa.
Your public key has been saved in /home/user/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:abcdef1234567890... user@localhost
The key's randomart image is:
+---[RSA 4096]----+
|        .o.    . |
|         o .  . .|
|        . . .o + |
|       . . o+E+ .|
|      . S . +=oo |
|       . . ...+..|
|        . .   o.o|
|         .   . o=|
|          .  . .o|
+----[SHA256]-----+
```

**Generate with specific parameters (non-interactive):**
```bash
ssh-keygen -t ed25519 -C "user@example.com" -f ~/.ssh/id_ed25519 -N "passphrase"
```

**Parameters:**
- `-t` - Key type (rsa, dsa, ecdsa, ed25519)
- `-C` - Comment (usually email)
- `-f` - Output file path
- `-N` - Passphrase (empty = no passphrase)
- `-b` - Key size (4096 for RSA)

**Recommended: Use Ed25519 (modern)**
```bash
ssh-keygen -t ed25519 -C "user@example.com"
```

---

## Copy SSH Key to Server

**Automated copy:**
```bash
ssh-copy-id user@server-ip
```

**Expected output:**
```
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/user/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to connect to host "server-ip"
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you would like to install them now -- use "-ssh-copy-id -f" to install them
user@server-ip's password:

Number of key(s) added: 1

Now try logging in with:   "ssh 'user@server-ip'"
and check to make sure that only the keys you wanted were added.
```

**Manual copy:**
```bash
cat ~/.ssh/id_rsa.pub | ssh user@server-ip "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

**After adding key, verify login works without password:**
```bash
ssh user@server-ip
# Should connect without prompting for password
```

---

## SSH Key Permissions

**Set correct permissions (critical):**
```bash
chmod 700 ~/.ssh              # Directory
chmod 600 ~/.ssh/id_rsa       # Private key
chmod 644 ~/.ssh/id_rsa.pub   # Public key
chmod 600 ~/.ssh/authorized_keys  # On server
```

**Verify permissions:**
```bash
ls -la ~/.ssh/
```

**Expected output:**
```
drwx------  2 user user 4096 Jan 16 10:30 .ssh
-rw-------  1 user user 3243 Jan 16 10:30 id_rsa
-rw-r--r--  1 user user  751 Jan 16 10:30 id_rsa.pub
-rw-------  1 user user 2048 Jan 16 10:30 authorized_keys
```

---

## List and Manage Keys

**List public keys:**
```bash
cat ~/.ssh/id_rsa.pub
```

**Expected output:**
```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC... user@localhost
```

**View key fingerprint:**
```bash
ssh-keygen -l -f ~/.ssh/id_rsa.pub
```

**Expected output:**
```
4096 SHA256:abcdef1234567890xyzAbcDef... user@localhost (RSA)
```

---

# SSH Configuration

## SSH Config File

**Create custom SSH configurations:**
```bash
nano ~/.ssh/config
```

**Example configuration file:**
```ini
# Default settings
Host *
    AddKeysToAgent yes
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
    ServerAliveCountMax 3

# Work servers
Host webserver
    HostName web.example.com
    User deploy
    Port 2222
    IdentityFile ~/.ssh/work_key

# Database server with jump host
Host dbserver
    HostName 10.0.1.50
    User dbadmin
    ProxyJump jumphost
    IdentityFile ~/.ssh/db_key

# Jump host (bastion)
Host jumphost
    HostName jump.example.com
    User admin
    IdentityFile ~/.ssh/jump_key
```

**Usage:**
```bash
# Connect using config alias
ssh webserver

# Execute command and exit
ssh webserver "ls -la /home/deploy"

# Copy file using config alias
scp webserver:/data/file.txt ~/
```

**Config file permissions:**
```bash
chmod 600 ~/.ssh/config
```

---

## Server-Side SSH Configuration

**Edit SSH server configuration:**
```bash
sudo nano /etc/ssh/sshd_config
```

**Security-focused configuration:**
```ini
# Network
Port 2222
AddressFamily any
ListenAddress 0.0.0.0

# Authentication
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
PermitEmptyPasswords no
X11Forwarding no

# Timeouts
ClientAliveInterval 300
ClientAliveCountMax 2

# Limits
MaxAuthTries 3
MaxSessions 10
MaxStartups 10:30:100

# Logging
SyslogFacility AUTH
LogLevel VERBOSE
```

**Restart SSH service:**
```bash
sudo systemctl restart sshd
```

**Test new configuration (before restart):**
```bash
sudo sshd -t
```

---

# Port Forwarding

## Local Port Forwarding

**Forward local port to remote service:**
```bash
ssh -L 3306:localhost:3306 user@server.com
```

**Usage:** Connect to localhost:3306 to reach MySQL on remote server

**Example with config:**
```bash
# Add to ~/.ssh/config
Host mysql-tunnel
    HostName database.example.com
    User dbadmin
    LocalForward 3306 localhost:3306
```

## Remote Port Forwarding

**Expose local service to remote network:**
```bash
ssh -R 8080:localhost:8080 user@remote-server.com
```

**Usage:** Remote server can access localhost:8080 from your machine

## Dynamic Port Forwarding (SOCKS Proxy)

**Create SOCKS proxy:**
```bash
ssh -D 1080 user@server.com
```

**Configure browser to use SOCKS proxy:**
- Proxy type: SOCKS5
- Host: localhost
- Port: 1080

---

# Security Best Practices

## Disable Password Authentication

**On server, edit /etc/ssh/sshd_config:**
```bash
PasswordAuthentication no
PubkeyAuthentication yes
```

**Restart SSH:**
```bash
sudo systemctl restart sshd
```

## Use Strong Key Types

**Recommended order:**
1. Ed25519 (modern, fast, secure)
2. ECDSA (good alternative)
3. RSA 4096-bit (if needed for compatibility)

**Avoid:**
- RSA 2048-bit (too weak)
- DSA (deprecated)

## Key Passphrase

**Generate key with passphrase:**
```bash
ssh-keygen -t ed25519 -N "strong-passphrase"
```

**Benefits:**
- Protects key if stolen
- Requires passphrase entry (or use ssh-agent)

## SSH Agent

**Start SSH agent:**
```bash
eval $(ssh-agent -s)
```

**Add key to agent:**
```bash
ssh-add ~/.ssh/id_ed25519
```

**List loaded keys:**
```bash
ssh-add -l
```

---

# Firewall Configuration

## UFW - Uncomplicated Firewall

**Check firewall status:**
```bash
sudo ufw status
```

**Expected output:**
```
Status: active

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW       Anywhere
80/tcp                     ALLOW       Anywhere
443/tcp                     ALLOW       Anywhere
22/tcp (v6)                ALLOW       Anywhere (v6)
80/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)
```

**Enable firewall:**
```bash
sudo ufw enable
```

**Allow SSH (do this FIRST before enabling firewall!):**
```bash
sudo ufw allow 22/tcp
```

**Allow specific port:**
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

**Allow port range:**
```bash
sudo ufw allow 5000:6000/tcp
```

**Allow from specific IP:**
```bash
sudo ufw allow from 192.168.1.0/24 to any port 22
```

**Deny port:**
```bash
sudo ufw deny 3306
```

**Delete rule:**
```bash
sudo ufw delete allow 3306
```

**Reload firewall:**
```bash
sudo ufw reload
```

---

## iptables - Advanced Firewall

**View current rules:**
```bash
sudo iptables -L -n
```

**Expected output:**
```
Chain INPUT (policy DROP)
target     prot opt source               destination
ACCEPT     all  --  0.0.0.0/0            0.0.0.0/0            state RELATED,ESTABLISHED
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:22
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80
```

**Allow SSH:**
```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

**Allow established connections:**
```bash
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

**Set default policy:**
```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

**Save rules (persistent):**
```bash
sudo iptables-save > /etc/iptables/rules.v4
```

---

## Fail2Ban - Brute Force Protection

**Check Fail2Ban status:**
```bash
sudo systemctl status fail2ban
```

**View jail configuration:**
```bash
sudo fail2ban-client status
```

**Expected output:**
```
|- Number of jail:	2
`- Jail list:	sshd, recidive
```

**Check specific jail:**
```bash
sudo fail2ban-client status sshd
```

**Expected output:**
```
Status for the jail: sshd
|- Filter currently enabled: True
|- Currently failed:	5
|- Currently banned:	2
`- Total banned:	15
```

**Manually ban IP:**
```bash
sudo fail2ban-client set sshd banip 203.0.113.100
```

**Unban IP:**
```bash
sudo fail2ban-client set sshd unbanip 203.0.113.100
```

**Configure Fail2Ban:**
```bash
sudo nano /etc/fail2ban/jail.local
```

**Example configuration:**
```ini
[DEFAULT]
bantime = 3600
findtime = 600
maxretry = 5

[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
```

**Restart Fail2Ban:**
```bash
sudo systemctl restart fail2ban
```

---

# Security Checklist

- [ ] SSH key pair generated and copied to servers
- [ ] PasswordAuthentication disabled
- [ ] Root login disabled (PermitRootLogin no)
- [ ] SSH running on non-standard port (optional)
- [ ] Firewall enabled and properly configured
- [ ] Fail2Ban installed and running
- [ ] Key permissions set correctly (700 for ~/.ssh, 600 for keys)
- [ ] SSH config file optimized for security
- [ ] Regular backups of SSH keys
- [ ] ssh-agent configured for passphrase-protected keys

---

# Common Issues

## "Permission denied (publickey)"

**Check causes:**
```bash
# 1. Verify key is added to agent
ssh-add -l

# 2. Verify authorized_keys on server
ssh user@server "cat ~/.ssh/authorized_keys"

# 3. Check server permissions
ssh user@server "ls -la ~/.ssh/"

# 4. Check SSH server logs
ssh user@server "sudo tail -50 /var/log/auth.log"
```

## Can't connect after changing SSH port

**Connect using new port:**
```bash
ssh -p 2222 user@server
```

**Test before restart:**
```bash
sudo sshd -t -p 2222
```

---

# Best Practices

1. **Use key-based authentication** - Disable password login
2. **Use Ed25519 keys** - Modern, faster, equally secure
3. **Protect private keys** - Use passphrases and proper permissions
4. **Disable root SSH** - Use sudo instead
5. **Use non-standard port** - Reduces noise from automated attacks
6. **Enable Fail2Ban** - Protect against brute force
7. **Keep keys backed up** - Encrypt and store securely
8. **Rotate keys regularly** - Every 1-2 years
9. **Monitor SSH logs** - Alert on suspicious activity
10. **Use SSH ProxyJump** - For accessing internal networks securely

---

# Summary

SSH security is fundamental to Linux system administration. Use key-based authentication, configure strong firewall rules, enable intrusion protection, and follow security best practices to create a secure environment.