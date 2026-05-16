# systemd

systemd is the service manager in modern Linux systems.

---

# Service Management

## Start Service

```bash
sudo systemctl start nginx
```

---

## Stop Service

```bash
sudo systemctl stop nginx
```

---

## Restart Service

```bash
sudo systemctl restart nginx
```

---

## Enable Service

Start service during boot.

```bash
sudo systemctl enable nginx
```

---

## Disable Service

```bash
sudo systemctl disable nginx
```

---

# Check Service Status

```bash
systemctl status nginx
```

---

# View Logs

```bash
journalctl -u nginx
```

---

# List Services

```bash
systemctl list-units --type=service
```

---

# Boot Targets

Check default target.

```bash
systemctl get-default
```

---

# Summary

systemd manages Linux services and startup processes.