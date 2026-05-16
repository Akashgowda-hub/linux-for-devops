# Logs & Monitoring

Logs help troubleshoot Linux systems.

---

# Important Log Files

| File | Purpose |
|------|---------|
| /var/log/syslog | System logs |
| /var/log/auth.log | Authentication logs |
| /var/log/kern.log | Kernel logs |

---

# View Logs

## tail -f

Monitor logs in real time.

```bash
tail -f /var/log/syslog
```

---

## grep

Search logs.

```bash
grep error /var/log/syslog
```

---

# System Monitoring

## top

Real-time process monitoring.

```bash
top
```

---

## htop

Interactive monitoring tool.

```bash
htop
```

---

## free -h

Check memory usage.

```bash
free -h
```

---

## df -h

Check disk usage.

```bash
df -h
```

---

## uptime

System uptime and load.

```bash
uptime
```

---

# Network Monitoring

## ss -tulpn

Show listening ports.

```bash
ss -tulpn
```

---

# Summary

Monitoring tools help identify system performance issues.