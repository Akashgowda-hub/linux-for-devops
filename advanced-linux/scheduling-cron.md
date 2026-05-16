# Cron Jobs & Task Scheduling

Cron is the classical task scheduler in Linux, allowing you to run commands at specified times. Modern systemd timers provide an alternative with better logging and integration.

---

# Cron Basics

## Cron Format

Complete cron expression syntax:

```text
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of week (0 - 7) (0 and 7 are Sunday)
│ │ │ │ │
│ │ │ │ │
* * * * * command to execute
```

**Special symbols:**
- `*` - Any value
- `,` - Multiple values (1,3,5)
- `-` - Range (1-5)
- `/` - Step values (*/5 = every 5)
- `?` - No specific value (day/weekday conflicts)

---

## Cron Expression Examples

**Every minute:**
```bash
* * * * * /path/to/command
```

**Every 5 minutes:**
```bash
*/5 * * * * /path/to/command
```

**Every 30 minutes:**
```bash
0,30 * * * * /path/to/command
# or
*/30 * * * * /path/to/command
```

**Every hour:**
```bash
0 * * * * /path/to/command
```

**Every 2 hours:**
```bash
0 */2 * * * /path/to/command
```

**Daily at 2 AM:**
```bash
0 2 * * * /path/to/backup.sh
```

**Daily at 9:30 AM:**
```bash
30 9 * * * /path/to/command
```

**Every day at midnight:**
```bash
0 0 * * * /path/to/command
```

**Every Monday at 8 AM:**
```bash
0 8 * * 1 /path/to/command
```

**Weekdays (Mon-Fri) at 5 PM:**
```bash
0 17 * * 1-5 /path/to/command
```

**Every 1st of the month at 6 AM:**
```bash
0 6 1 * * /path/to/command
```

**Every 1st and 15th of month at noon:**
```bash
0 12 1,15 * * /path/to/command
```

**January 1st at midnight (New Year):**
```bash
0 0 1 1 * /path/to/newyear.sh
```

**Every Monday, Wednesday, Friday at 10 AM:**
```bash
0 10 * * 1,3,5 /path/to/command
```

**Q1 (Jan, Feb, Mar) every day at 3 AM:**
```bash
0 3 * 1-3 * /path/to/quarterly-task.sh
```

**Last day of month at 11:59 PM:**
```bash
59 23 L * * /path/to/command
# Note: Not all cron implementations support 'L' - better to use:
0 0 1 * * /path/to/command  # 1st of next month
```

---

# Crontab - User Cron Jobs

## Edit Cron Jobs

**Open crontab editor:**
```bash
crontab -e
```

**Expected:** Opens in your default editor (vim, nano, etc.)

**Example crontab file:**
```bash
# Backup database daily at 2 AM
0 2 * * * /usr/local/bin/backup-mysql.sh >> /var/log/backup.log 2>&1

# Clear temp files every Sunday at 3 AM
0 3 * * 0 rm -f /tmp/*.tmp

# Monitor disk space every hour
0 * * * * /usr/local/bin/check-disk.sh

# Update system packages daily at 4 AM
0 4 * * * apt-get update && apt-get upgrade -y

# Run Python script every 30 minutes
*/30 * * * * /usr/bin/python3 /home/user/script.py
```

---

## List Cron Jobs

**View your cron jobs:**
```bash
crontab -l
```

**Expected output:**
```
# Backup database daily at 2 AM
0 2 * * * /usr/local/bin/backup-mysql.sh >> /var/log/backup.log 2>&1

# Clear temp files every Sunday at 3 AM
0 3 * * 0 rm -f /tmp/*.tmp
```

**View another user's crontab (as root):**
```bash
sudo crontab -u username -l
```

---

## Remove Cron Jobs

**Remove all cron jobs:**
```bash
crontab -r
```

**Remove specific job:**
```bash
crontab -e  # Then delete the line
```

---

## Cron Job Best Practices

**Redirect output to log files:**
```bash
# GOOD: Logs to file
0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1

# BAD: Output lost, only errors shown
0 2 * * * /usr/local/bin/backup.sh

# BEST: Include timestamp and status
0 2 * * * echo "$(date): Starting backup..." >> /var/log/backup.log && /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1
```

**Use full paths:**
```bash
# GOOD
0 2 * * * /usr/bin/python3 /home/user/script.py

# BAD (PATH may not be set in cron)
0 2 * * * python3 /home/user/script.py
```

**Use environment variables:**
```bash
# At top of crontab
MAILTO=admin@example.com
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin

# Then use simpler commands
0 2 * * * backup.sh
```

---

# System Cron - /etc/crontab

For system-wide cron jobs (run as specific user).

```bash
sudo nano /etc/crontab
```

**Example /etc/crontab:**
```bash
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# Regular expression matchers for cron entries
# @yearly (or @annually)
# @monthly
# @weekly
# @daily (or @midnight)
# @hourly

# m h  dom mon dow user   command
0 2    *   *   *   root   /usr/local/bin/backup.sh
0 */4  *   *   *   root   /usr/local/bin/cleanup.sh
0 9    *   *   1   www-data /usr/local/bin/report.sh
```

**Key difference from user crontab:**
- Requires specifying the user to run as
- Better for system maintenance tasks

---

## Cron Directories

**Hourly, daily, weekly, monthly cron:**
```bash
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
```

**Add scripts:**
```bash
# Create executable script
sudo nano /etc/cron.daily/my-backup
#!/bin/bash
/usr/local/bin/backup.sh

# Make executable
sudo chmod +x /etc/cron.daily/my-backup
```

---

# Common Cron Jobs

## Backup Example

```bash
# Daily backup at 2 AM
0 2 * * * /usr/local/bin/backup.sh

# File: /usr/local/bin/backup.sh
#!/bin/bash
BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d)
tar -czf $BACKUP_DIR/backup-$DATE.tar.gz /home /etc
```

## Log Rotation Example

```bash
# Rotate logs daily at midnight
0 0 * * * logrotate /etc/logrotate.conf
```

## System Maintenance Example

```bash
# Clean temp files daily at 3 AM
0 3 * * * find /tmp -type f -atime +7 -delete

# Update package list daily at 4 AM
0 4 * * * apt-get update

# Security updates daily at 5 AM
0 5 * * * apt-get upgrade -y
```

## Monitoring Example

```bash
# Check disk space hourly
0 * * * * /usr/local/bin/check-disk-usage.sh

# Monitor system load every 10 minutes
*/10 * * * * /usr/local/bin/check-load.sh
```

---

# Debugging Cron Jobs

## Check if Cron Service is Running

```bash
sudo systemctl status cron
# or on some systems:
sudo systemctl status crond
```

**Expected output:**
```
● cron.service - Regular background program processing daemon
     Loaded: loaded (/lib/systemd/system/cron.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2024-01-16 10:30:45 UTC; 5 days ago
```

## View Cron Logs

**On systems with systemd:**
```bash
journalctl -u cron
journalctl -u cron -f  # Follow in real-time
```

**Traditional syslog:**
```bash
grep CRON /var/log/syslog
tail -f /var/log/syslog | grep CRON
```

**Expected output:**
```
Jan 16 02:00:01 server CRON[12345]: (root) CMD (/usr/local/bin/backup.sh)
Jan 16 02:15:30 server CRON[12346]: (root) CMD (/usr/local/bin/cleanup.sh)
```

## Test Cron Command Manually

```bash
# Run the command directly
/usr/local/bin/backup.sh

# Check exit code
echo $?  # 0 = success, non-zero = error
```

## Common Cron Problems

**Problem: Command doesn't run**
```bash
# Check cron is running
sudo systemctl start cron

# Check syntax of crontab
crontab -l

# Check permissions
ls -la /usr/local/bin/backup.sh  # Must be executable

# Verify command has full path
# DON'T: 0 2 * * * backup.sh (won't find it)
# DO:    0 2 * * * /usr/local/bin/backup.sh
```

**Problem: Output not captured**
```bash
# DON'T: 0 2 * * * backup.sh (output lost)
# DO:    0 2 * * * backup.sh >> /var/log/backup.log 2>&1
```

**Problem: Environment variables missing**
```bash
# Check what environment cron has:
0 * * * * env > /tmp/cron-env.txt

# In crontab, set needed variables:
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin
SHELL=/bin/bash
0 2 * * * backup.sh
```

---

# Systemd Timers (Modern Alternative)

systemd timers provide a modern alternative to cron with better logging and dependencies.

## Creating a Systemd Timer

**Create service file:**
```bash
sudo nano /etc/systemd/system/backup.service
```

**Content:**
```ini
[Unit]
Description=Daily Backup Service
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup.sh
StandardOutput=journal
StandardError=journal
```

**Create timer file:**
```bash
sudo nano /etc/systemd/system/backup.timer
```

**Content:**
```ini
[Unit]
Description=Daily Backup Timer
Requires=backup.service

[Timer]
OnCalendar=daily
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

**Enable and start:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable backup.timer
sudo systemctl start backup.timer
```

## Monitor Timers

```bash
# List all timers
systemctl list-timers

# Check specific timer
systemctl status backup.timer

# View timer logs
journalctl -u backup.service
```

---

# Cron vs Systemd Timers

| Feature | Cron | Systemd Timer |
|---------|------|---------------|
| Syntax | cron expression | Calendar format |
| Logging | To syslog | To journal |
| Dependencies | Limited | Full support |
| Real-time monitoring | Limited | Yes (list-timers) |
| Accuracy | Per minute | Configurable |
| Persistent jobs | No | Yes (Persistent=true) |

---

# Best Practices

1. **Always use full paths** - `/usr/bin/python3` not `python3`
2. **Redirect output** - Log to file or syslog
3. **Set environment** - PATH and SHELL at crontab top
4. **Use mail** - Set MAILTO for error notifications
5. **Monitor execution** - Check logs regularly
6. **Test commands** - Run manually before automating
7. **Use meaningful names** - Comment what each job does
8. **Consider systemd timers** - For new installations
9. **Avoid heavy operations** - Don't run during peak hours
10. **Backup crontabs** - Keep version controlled

---

# Summary

Cron provides reliable task scheduling through simple expressions and automatic execution. While systemd timers offer modern alternatives with better integration, cron remains the standard for Linux task scheduling due to its simplicity and universal availability.