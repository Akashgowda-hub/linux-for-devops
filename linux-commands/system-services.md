# System Services

## systemctl - Service Management with systemd

Systemctl is the primary tool for managing systemd services and units on modern Linux systems.

### Check Service Status

**Basic status check:**
```bash
systemctl status nginx
```

**Output:**
```
● nginx.service - The NGINX HTTP and Reverse Proxy Server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
     Active: active (running) since Mon 2024-01-08 10:30:45 UTC; 2 days ago
   Process: 1234 ExecStartPre=/usr/sbin/nginx -t (code=exited, status=0/SUCCESS)
  Main PID: 1235 (nginx)
    CGroup: /system.slice/nginx.service
            ├─1235 nginx: master process
            └─1236 nginx: worker process
```

### Common Use Cases

**Start a service:**
```bash
sudo systemctl start nginx
```

**Stop a service:**
```bash
sudo systemctl stop nginx
```

**Restart a service:**
```bash
sudo systemctl restart nginx
```

**Reload service configuration (without stopping):**
```bash
sudo systemctl reload nginx
```

**Enable service at boot:**
```bash
sudo systemctl enable nginx
# Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service
```

**Disable service at boot:**
```bash
sudo systemctl disable nginx
# Removed /etc/systemd/system/multi-user.target.wants/nginx.service
```

**Check if service is enabled:**
```bash
systemctl is-enabled nginx
```
**Output:**
```
enabled
```

**Check if service is running:**
```bash
systemctl is-active nginx
```
**Output:**
```
active
```

---

## List All Services

**Show all available services:**
```bash
systemctl list-unit-files --type=service
```

**Output (abbreviated):**
```
UNIT FILE                              STATE
nginx.service                          enabled
ssh.service                            enabled
docker.service                         enabled
mysql.service                          disabled
postgresql.service                    disabled
```

### Common Use Cases

**List running services only:**
```bash
systemctl list-units --type=service --state=running
```

**List failed services:**
```bash
systemctl list-units --type=service --state=failed
```

**List enabled services:**
```bash
systemctl list-units --type=service --state=enabled
```

**Check all services and their status:**
```bash
systemctl list-units --type=service --all
```

**Search for specific service:**
```bash
systemctl list-units --type=service | grep mysql
```

---

## Service Logs with journalctl

View logs for system services using journalctl.

### View Service Logs

**Recent logs for service:**
```bash
journalctl -u nginx
```

**Output (abbreviated):**
```
Jan 08 10:30:45 server systemd[1]: Starting The NGINX HTTP and Reverse Proxy Server...
Jan 08 10:30:45 server systemd[1]: Started The NGINX HTTP and Reverse Proxy Server.
Jan 08 10:40:12 server nginx[1235]: worker process exited with code 1
Jan 08 10:40:12 server systemd[1]: nginx.service: Main process exited, code=exited, status=1/FAILURE
```

### Common Use Cases

**Show last 50 lines:**
```bash
journalctl -u nginx -n 50
```

**Show logs since last boot:**
```bash
journalctl -u nginx -b
```

**Show logs from specific time:**
```bash
journalctl -u nginx --since "2024-01-08 10:00:00" --until "2024-01-08 11:00:00"
```

**Follow logs in real-time (like tail -f):**
```bash
journalctl -u nginx -f
```

**Show only errors:**
```bash
journalctl -u nginx -p err
```

**Show logs with timestamps:**
```bash
journalctl -u nginx --no-pager
```

**Count log entries:**
```bash
journalctl -u nginx | wc -l
```

---

## Real-Time Log Monitoring

### Monitor Log File

**Tail logs in real-time:**
```bash
tail -f /var/log/syslog
```

**Output (continuously updating):**
```
Jan 8 10:30:45 server systemd: Starting... 
Jan 8 10:30:46 server systemd: Started...
Jan 8 10:31:02 server kernel: [1234.567] Out of memory: Kill process...
```

### Common Use Cases

**Monitor with line numbers:**
```bash
tail -nf /var/log/syslog
```

**Show last 100 lines and follow:**
```bash
tail -n 100 -f /var/log/syslog
```

**Monitor specific pattern in logs:**
```bash
tail -f /var/log/syslog | grep "ERROR"
```

**Monitor with timestamps:**
```bash
tail -f /var/log/syslog | while IFS= read line; do echo "$(date '+%H:%M:%S') $line"; done
```

**Monitor multiple log files:**
```bash
tail -f /var/log/syslog /var/log/auth.log
```

**Monitor with color highlighting:**
```bash
tail -f /var/log/syslog | grep --color=auto "ERROR\|WARNING"
```

---

## Check Installed Software

**List all installed packages (Ubuntu/Debian):**
```bash
apt list --installed
```

**Output (abbreviated):**
```
adduser/jammy,now 3.118ubuntu2 all [installed]
apparmor/jammy-security,now 3.0.4-2ubuntu2.3 amd64 [installed]
base-files/jammy-updates,now 12.3.7ubuntu1 amd64 [installed]
ca-certificates/jammy,now 20230311 all [installed]
curl/jammy-updates,now 7.81.0-1ubuntu1.10 amd64 [installed]
```

### Common Use Cases

**Count total installed packages:**
```bash
apt list --installed | wc -l
```

**Search for specific package:**
```bash
apt list --installed | grep nginx
```

**Show detailed info about package:**
```bash
apt show nginx
```

**List recent updates:**
```bash
apt list --upgradable
```

**List packages with status:**
```bash
dpkg -l | head -20
```

**For RedHat/CentOS systems:**
```bash
rpm -qa | head -20
```

**Find where package file is installed:**
```bash
dpkg -L nginx | grep bin
```

---

## Service Management Workflow Examples

### Before & After: Starting a Web Server

**Before - Manual service management:**
```bash
# Check if service exists
ls /etc/init.d/

# Try to start
/etc/init.d/nginx start

# Check if running
ps aux | grep nginx
```

**After - Using systemctl:**
```bash
# Start and enable
sudo systemctl start nginx
sudo systemctl enable nginx

# Verify
systemctl status nginx
```

### Example: Debugging Service Failures

**Scenario: Service won't start**

```bash
# Step 1: Check status
sudo systemctl status nginx
# Output shows: "failed"

# Step 2: View error logs
sudo journalctl -u nginx -n 20

# Step 3: Check service configuration syntax
sudo nginx -t
# Output shows: "syntax error on line 5"

# Step 4: Fix configuration

# Step 5: Try starting again
sudo systemctl restart nginx

# Step 6: Verify
sudo systemctl status nginx
```

### Example: Monitor Service After Restart

```bash
# Restart service
sudo systemctl restart mysql

# Watch logs
journalctl -u mysql -f

# In another terminal, check status
watch -n 1 'systemctl status mysql | grep Active'
```

---

## Service File Location and Management

**System service files:**
```bash
ls /etc/systemd/system/*.service
```

**User service files:**
```bash
ls ~/.config/systemd/user/*.service
```

**Reload systemd daemon after service changes:**
```bash
sudo systemctl daemon-reload
```

**Enable service for current user:**
```bash
systemctl --user enable service-name
```

---

## Best Practices

1. **Always check status before troubleshooting:**
   ```bash
   systemctl status service-name
   ```

2. **Enable services you want to persist after reboot:**
   ```bash
   sudo systemctl enable service-name
   ```

3. **Use reload instead of restart when possible:**
   ```bash
   sudo systemctl reload nginx  # Faster, no downtime for reloads
   ```

4. **Check logs for failures:**
   ```bash
   journalctl -u service-name -p err
   ```

5. **Test configuration before restarting:**
   ```bash
   # For nginx
   sudo nginx -t
   
   # Then reload
   sudo systemctl reload nginx
   ```

6. **Monitor service after changes:**
   ```bash
   journalctl -u service-name -f
   ```