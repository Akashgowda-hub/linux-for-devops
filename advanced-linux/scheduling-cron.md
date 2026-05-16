# Cron Jobs

Cron is used for scheduling tasks in Linux.

---

# Edit Cron Jobs

```bash
crontab -e
```

---

# List Cron Jobs

```bash
crontab -l
```

---

# Cron Format

```text
* * * * * command
- - - - -
| | | | |
| | | | +---- Day of Week
| | | +------ Month
| | +-------- Day
| +---------- Hour
+------------ Minute
```

---

# Example Cron Jobs

## Run every day at 2 AM

```bash
0 2 * * * backup.sh
```

---

## Run every 5 minutes

```bash
*/5 * * * * script.sh
```

---

# System Cron

```bash
/etc/crontab
```

---

# Check Cron Service

```bash
systemctl status cron
```

---

# Summary

Cron automates repetitive Linux tasks.