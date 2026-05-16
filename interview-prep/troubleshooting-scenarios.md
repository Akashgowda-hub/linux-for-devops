# Troubleshooting Scenarios

Common Linux troubleshooting scenarios for DevOps engineers.

---

# High CPU Usage

## Check Processes

```bash
top
```

---

## Kill Process

```bash
kill -9 PID
```

---

# Disk Full

## Check Disk Usage

```bash
df -h
```

---

## Find Large Files

```bash
du -sh /*
```

---

# Service Not Starting

## Check Status

```bash
systemctl status nginx
```

---

## View Logs

```bash
journalctl -u nginx
```

---

# Port Already in Use

## Check Port

```bash
ss -tulpn
```

---

# Permission Denied

## Check Permissions

```bash
ls -l
```

---

## Change Permissions

```bash
chmod 755 file.sh
```

---

# DNS Issues

## Test DNS

```bash
nslookup google.com
```

---

## Check Connectivity

```bash
ping google.com
```

---

# Summary

Troubleshooting skills are essential for Linux and DevOps roles.