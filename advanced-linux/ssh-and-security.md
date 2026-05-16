# SSH & Security

SSH provides secure remote access to Linux systems.

---

# SSH Login

```bash
ssh user@server-ip
```

---

# Generate SSH Key

```bash
ssh-keygen
```

---

# Copy SSH Key

```bash
ssh-copy-id user@server-ip
```

---

# SSH Config File

```bash
~/.ssh/config
```

---

# File Permissions

Private key permissions:

```bash
chmod 600 ~/.ssh/id_rsa
```

---

# Firewall

## UFW Status

```bash
sudo ufw status
```

---

## Allow Port

```bash
sudo ufw allow 22
```

---

# iptables

View firewall rules.

```bash
sudo iptables -L
```

---

# Fail2Ban

Protect against brute force attacks.

```bash
sudo systemctl status fail2ban
```

---

# Summary

SSH and firewall configuration are essential for Linux server security.