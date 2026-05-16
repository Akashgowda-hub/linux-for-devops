# System Services

## systemctl status

Check service status.

```bash
systemctl status nginx
```

---

## systemctl start

Start service.

```bash
sudo systemctl start nginx
```

---

## systemctl stop

Stop service.

```bash
sudo systemctl stop nginx
```

---

## systemctl restart

Restart service.

```bash
sudo systemctl restart nginx
```

---

## Find all installed services

Installed services.

```bash
systemctl list-unit-files --type=service
```

---


## journalctl

View service logs.

```bash
journalctl -u nginx
```

---

## tail -f

Monitor logs in real time.

```bash
tail -f /var/log/syslog
```

## Check installed software using package manager

Installed software.

```bash
apt list --installed
```

---