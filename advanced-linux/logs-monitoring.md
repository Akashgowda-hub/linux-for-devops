# Logs & Monitoring

Comprehensive logging and monitoring are essential for system troubleshooting, performance analysis, and security. Linux provides powerful tools for viewing logs, filtering events, and monitoring system resources in real-time.

---

# Important Log Files

| File | Purpose | Typical Content |
|------|---------|-----------------|
| /var/log/syslog | System logs (Debian/Ubuntu) | General system events, services |
| /var/log/messages | System logs (RedHat/CentOS) | General system events |
| /var/log/auth.log | Authentication logs | Login attempts, sudo commands |
| /var/log/kern.log | Kernel logs | Kernel messages, hardware events |
| /var/log/dmesg | Boot messages | Hardware detection, driver info |
| /var/log/apache2/access.log | Web server access | HTTP requests |
| /var/log/apache2/error.log | Web server errors | Application errors |
| /var/log/nginx/error.log | Nginx errors | Nginx errors and warnings |

---

# journalctl - Unified System Logging

systemd's `journalctl` is the modern way to view logs with powerful filtering capabilities.

## Basic Log Viewing

**View last 100 lines:**
```bash
journalctl -n 100
```

**Follow logs in real-time (like tail -f):**
```bash
journalctl -f
```

**Expected output:**
```
Jan 16 10:30:45 server systemd[1]: Started User Manager for UID 1000.
Jan 16 10:30:46 server NetworkManager[1234]: <info> Activation (docker0)started (device bound).
Jan 16 10:30:47 server kernel: audit: type=1400 audit(1705406447.123:567): apparmor="DENIED"
Jan 16 10:30:48 server sshd[5678]: Accepted publickey for user from 192.168.1.100 port 54321 ssh2
```

---

## Filtering by Service

**View logs for specific service:**
```bash
journalctl -u nginx
```

**Follow service logs in real-time:**
```bash
journalctl -u nginx -f
```

**Example output:**
```
Jan 16 10:30:45 server nginx[12345]: Starting A high performance web server...
Jan 16 10:30:46 server nginx[12345]: Configuration test passed.
Jan 16 10:30:46 server systemd[1]: Started nginx.service.
```

---

## Filtering by Priority

**View only errors and critical messages:**
```bash
# Priority levels: 0-7 (emerg, alert, crit, err, warning, notice, info, debug)
journalctl -p err
journalctl -p err::crit  # From error to critical
```

**Common priority filters:**
```bash
journalctl -p 0          # Emergencies only
journalctl -p 1          # Alerts only
journalctl -p err        # Errors and more severe
journalctl -p warning    # Warnings and more severe
journalctl -p info       # Info and more severe (most logs)
journalctl -p debug      # Everything
```

**Example - showing only warnings:**
```bash
journalctl -p warning -n 50
```

**Expected output:**
```
Jan 16 09:15:22 server kernel: Out of memory: Kill process nginx (12345)
Jan 16 09:45:33 server systemd[1]: postgresql.service: Failed with result 'exit-code'.
```

---

## Time-Based Filtering

**Logs since last boot:**
```bash
journalctl -b
```

**Logs since specific time:**
```bash
journalctl --since "2024-01-16 10:00:00"
journalctl --until "2024-01-16 11:00:00"
```

**Logs from last hour:**
```bash
journalctl --since "1 hour ago"
journalctl --since "-1h"
```

**Logs from today:**
```bash
journalctl --since "today"
```

**Example:**
```bash
# Between two times
journalctl --since "2024-01-16 09:00:00" --until "2024-01-16 12:00:00"
```

---

## Output Formatting

**Short format (default):**
```bash
journalctl -n 5
```

**Output:**
```
Jan 16 10:30:45 server systemd[1]: Started User Manager for UID 1000.
Jan 16 10:30:46 server nginx[12345]: Starting nginx service.
```

**Verbose output (full fields):**
```bash
journalctl -o verbose -n 5
```

**Output:**
```
Fri 2024-01-16 10:30:45.123456 UTC systemd[1]: Started User Manager for UID 1000.
  MESSAGE=Started User Manager for UID 1000.
  PRIORITY=6
  _PID=1
  _UID=0
  _SYSTEMD_UNIT=user-manager.service
```

**JSON format (for parsing):**
```bash
journalctl -o json -n 5
```

**One-line short format:**
```bash
journalctl -o short-iso -n 5
```

**Output:**
```
2024-01-16T10:30:45.123456+00:00 server systemd[1]: Started User Manager for UID 1000.
```

---

## Advanced Filtering

**Filter by command:**
```bash
journalctl /usr/sbin/nginx
```

**Filter by PID:**
```bash
journalctl _PID=12345
```

**Combine filters (AND logic):**
```bash
journalctl -u nginx -p err --since "1 hour ago"
```

**Search for keyword:**
```bash
journalctl -g "authentication failed"
```

---

## Practical Examples

**Find all failed login attempts in the last 24 hours:**
```bash
journalctl -u ssh -p err --since "24 hours ago"
```

**Monitor service for errors and warnings:**
```bash
journalctl -u postgresql -p warning -f
```

**Export logs to file:**
```bash
journalctl -u nginx --since "today" > /tmp/nginx-logs.txt
```

**Find when service crashed:**
```bash
journalctl -u myapp -p err -n 100
```

---

# Traditional Log Viewing

## tail - View End of File

**Show last 20 lines:**
```bash
tail -n 20 /var/log/syslog
```

**Follow logs in real-time:**
```bash
tail -f /var/log/syslog
```

**Expected output:**
```
Jan 16 10:30:45 server kernel: audit: type=1400 audit(...)
Jan 16 10:30:46 server systemd[1]: Started User Manager.
Jan 16 10:30:47 server sshd[5678]: Accepted publickey.
Jan 16 10:30:48 server sudo: user : TTY=pts/0 ; PWD=/home/user ; USER=root ; COMMAND=/usr/bin/systemctl
```

---

## grep - Search Logs

**Find error messages:**
```bash
grep -i error /var/log/syslog
```

**Find authentication failures:**
```bash
grep "failed password" /var/log/auth.log
```

**Count occurrences:**
```bash
grep -c "error" /var/log/syslog
```

**Show context (3 lines before/after):**
```bash
grep -C 3 "error" /var/log/syslog
```

**Find multiple patterns:**
```bash
grep -E "error|warning|critical" /var/log/syslog
```

**Example output:**
```
Jan 16 10:15:22 server nginx: error: connect to database failed
Jan 16 10:16:33 server kernel: error: out of memory
Jan 16 10:17:45 server systemd: error: service failed
```

---

# Log Rotation and Management

systemd-based systems automatically rotate logs through `logrotate`.

## Check Log Rotation Configuration

```bash
cat /etc/logrotate.conf
```

**Example output:**
```
# rotate log files daily
daily

# keep 7 days
rotate 7

# compress old logs
compress

# create new empty log after rotation
create

# don't rotate empty files
notifempty
```

## View Log Size

```bash
du -sh /var/log
ls -lh /var/log/syslog*
```

**Expected output:**
```
-rw-r--r-- 1 root root 2.3M Jan 16 10:30 /var/log/syslog
-rw-r--r-- 1 root root 1.2M Jan 15 00:00 /var/log/syslog.1
-rw-r--r-- 1 root root 891K Jan 14 00:00 /var/log/syslog.2.gz
```

## Clear Old Logs

```bash
# Delete logs older than 30 days
sudo journalctl --vacuum-time=30d

# Keep only 100MB of logs
sudo journalctl --vacuum-size=100M
```

---

# System Monitoring Tools

## top - Real-Time Process Monitoring

Interactive process viewer with CPU and memory information.

```bash
top
```

**Expected output:**
```
top - 10:35:22 up 5 days, 3:15, 2 users, load average: 0.85, 0.92, 0.88
Tasks: 185 total,   2 running, 183 sleeping,   0 stopped,   0 zombie
%Cpu(s):  8.2 us,  2.1 sy,  0.0 ni, 89.5 id,  0.0 wa,  0.0 hi,  0.2 si,  0.0 st
MiB Mem :   7953.7 total,   2341.2 free,   3210.5 used,   2401.9 buff/cache
MiB Swap:   2048.0 total,   2048.0 free,      0.0 used.   4201.3 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
12345 www-data  20   0  321.2m  45.2m   8.9m S  12.3  0.6   0:45.23 nginx
 5678 mysql     20   0 1.2g    521.4m  4.5m S   8.9  6.6   2:15.12 mysqld
 1234 root      20   0  234.5m  15.3m   6.7m S   2.1  0.2   0:12.34 systemd
```

**Key columns:**
- `PID` - Process ID
- `%CPU` - CPU usage percentage
- `%MEM` - Memory usage percentage
- `VIRT` - Total virtual memory
- `RES` - Physical memory in use
- `TIME+` - CPU time accumulated

**Interactive commands:**
- `P` - Sort by CPU usage
- `M` - Sort by memory usage
- `k` - Kill a process
- `r` - Change process priority
- `q` - Quit

---

## htop - Advanced Process Monitor

More user-friendly than `top` with visual enhancements.

```bash
htop
```

**Features:**
- Color-coded process hierarchy
- Easy process killing without PID lookup
- Tree view of process relationships
- Better memory visualization

---

## ps - Process Snapshot

View current processes (single snapshot, not real-time).

**List all processes with details:**
```bash
ps aux
```

**Expected output:**
```
USER   PID %CPU %MEM    VSZ   RSS TTY STAT START   TIME COMMAND
root     1  0.0  0.1  18204  1256 ?   Ss   10:20   0:01 /lib/systemd/systemd --system
root     2  0.0  0.0      0     0 ?   S    10:20   0:00 [kthreadd]
www-data 12345  2.3  0.5 321264 45215 ?   S   10:25   0:45 nginx: worker process
```

**Show process tree:**
```bash
ps auxf
```

---

## Memory Monitoring

**Check memory usage:**
```bash
free -h
```

**Expected output:**
```
              total        used        free      shared  buff/cache   available
Mem:          7.8Gi       2.3Gi       1.2Gi      512Mi       4.3Gi       4.8Gi
Swap:         2.0Gi       0.0Gi       2.0Gi
```

**Interpretation:**
- `total` - Total RAM
- `used` - Actively used memory
- `free` - Unused memory
- `available` - Memory available for new processes (includes cache)

**Detailed memory info:**
```bash
cat /proc/meminfo | head -20
```

---

## vmstat - Virtual Memory Statistics

Monitor system performance metrics including CPU, memory, disk I/O.

```bash
vmstat 1 5  # Sample every 1 second, 5 times
```

**Expected output:**
```
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 1234567 123456 3456789  0    0     0     0   45   12  5  2 92  1  0
 1  0      0 1200000 120000 3500000  0    0    10    15   50   15  8  3 88  1  0
```

**Key columns:**
- `r` - Processes in run queue (waiting for CPU)
- `b` - Processes blocked on I/O
- `us` - User CPU time (%)
- `sy` - System CPU time (%)
- `id` - Idle time (%)
- `wa` - Wait time for I/O (%)

---

## iostat - Disk I/O Statistics

Monitor disk performance.

```bash
iostat -x 1 3  # Extended stats, 1s interval, 3 samples
```

**Expected output:**
```
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           15.23    0.00    3.45    2.12    0.00   79.20

Device            r/s     w/s     rMB/s     wMB/s   %util
sda              12.34   45.67     0.45      1.23   8.90
sdb               2.10    8.45     0.12      0.34   2.15
```

**Key metrics:**
- `r/s` - Read operations per second
- `w/s` - Write operations per second
- `rMB/s` - Read throughput (MB/s)
- `wMB/s` - Write throughput (MB/s)
- `%util` - Disk utilization percentage

---

## Disk Usage

**Check disk space:**
```bash
df -h
```

**Expected output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   25G   22G  53% /
/dev/sda2       100G   45G   50G  47% /home
tmpfs           3.9G     0  3.9G   0% /dev/shm
```

**Check directory sizes:**
```bash
du -sh *
```

**Expected output:**
```
2.3G    var
1.2G    home
512M    opt
256M    tmp
```

**Find large files:**
```bash
find / -type f -size +100M -exec ls -lh {} \; | head -10
```

---

## Network Monitoring

**Check listening ports:**
```bash
ss -tulpn
```

**Expected output:**
```
State  Recv-Q Send-Q Local Address:Port Peer Address:Port Process
LISTEN     0    128 0.0.0.0:22          0.0.0.0:*          users:(("sshd",pid=1234,fd=3))
LISTEN     0    128 0.0.0.0:80          0.0.0.0:*          users:(("nginx",pid=5678,fd=6))
LISTEN     0    128 0.0.0.0:443         0.0.0.0:*          users:(("nginx",pid=5678,fd=7))
```

**Monitor network connections:**
```bash
watch -n 1 'ss -s'
```

---

## System Uptime and Load

**Check system uptime:**
```bash
uptime
```

**Expected output:**
```
 10:35:22 up 5 days, 3:15,  2 users,  load average: 0.85, 0.92, 0.88
```

**Interpretation:**
- `up 5 days, 3:15` - System has been running for 5 days and 3 hours
- `2 users` - Number of logged-in users
- `load average: 0.85, 0.92, 0.88` - Average load over 1, 5, and 15 minutes
  - If load > number of CPU cores, system is overloaded

---

# Real-Time Monitoring Techniques

## Monitor Multiple Metrics

**Watch resource usage update every second:**
```bash
watch -n 1 'free -h; echo "---"; ps aux --sort=-%cpu | head -5'
```

## Create Monitoring Dashboard

**Script to display all key metrics:**

```bash
#!/bin/bash
while true; do
  clear
  echo "=== System Monitoring Dashboard ==="
  echo ""
  echo "--- CPU ---"
  top -bn1 | grep "Cpu(s)" | sed "s/.*, *\([0-9.]*\)%* id.*/\1/" | awk '{print "CPU Usage: " 100-$1 "%"}'
  
  echo ""
  echo "--- Memory ---"
  free -h | grep Mem | awk '{print "Used: " $3 " / " $2}'
  
  echo ""
  echo "--- Disk ---"
  df -h / | tail -1 | awk '{print "Usage: " $3 " / " $2}'
  
  echo ""
  echo "--- Load Average ---"
  uptime | awk -F'load average:' '{print $2}'
  
  sleep 5
done
```

---

# Best Practices

1. **Set up log rotation** - Prevent logs from consuming all disk space
2. **Monitor in production** - Use continuous monitoring with alerting
3. **Keep logs centralized** - Use ELK stack or similar for distributed systems
4. **Set retention policies** - Delete old logs to save space
5. **Monitor critical services** - Alert on service failures immediately
6. **Track performance baselines** - Know normal vs abnormal behavior
7. **Use structured logging** - JSON logs for better parsing
8. **Review logs regularly** - Check for security events and errors

---

# Summary

Effective logging and monitoring are critical for system administration. Use `journalctl` for modern log viewing, combine multiple monitoring tools to get complete system visibility, and set up automated alerting for production systems.