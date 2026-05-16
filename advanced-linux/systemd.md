# systemd - Service Manager

systemd is the init system and service manager for modern Linux distributions. It replaces traditional SysVinit scripts and provides parallel service startup, socket activation, timer-based scheduling, and comprehensive dependency management.

---

# Service Management

## Start Service

Start a service immediately (doesn't persist after reboot).

```bash
sudo systemctl start nginx
```

**Expected output:** (no output on success)

```bash
# Verify it started
systemctl status nginx
```

**Expected output:**
```
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2024-01-16 10:30:45 UTC; 2min 5s ago
   Main PID: 12345 (nginx)
      Tasks: 2 (limit: 2353)
     Memory: 4.2M
        CPU: 125ms
     CGroup: /system.slice/nginx.service
             ├─12345 nginx: master process
             └─12346 nginx: worker process
```

---

## Stop Service

Stop a running service.

```bash
sudo systemctl stop nginx
```

**Verification:**
```bash
systemctl status nginx
```

**Expected output:**
```
○ nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Tue 2024-01-16 10:35:20 UTC; 5s ago
```

---

## Restart Service

Restart a service (graceful stop and start).

```bash
sudo systemctl restart nginx
```

**Best practice:** Use `restart` for applying configuration changes without losing connections (most services handle graceful restarts).

---

## Reload Service

Reload configuration without restarting (for services that support it).

```bash
sudo systemctl reload nginx
```

**Difference from restart:**
- `reload` - Reloads configuration, keeps running processes
- `restart` - Stops and starts fresh

**Expected output:** (none on success)

```bash
# Verify the reload worked
systemctl status nginx
```

---

## Enable Service

Enable service to start automatically during boot.

```bash
sudo systemctl enable nginx
```

**Expected output:**
```
Created symlink /etc/systemd/system/multi-user.target.wants/nginx.service → /lib/systemd/system/nginx.service.
```

**What happens:** Creates a symbolic link to start the service at boot.

---

## Disable Service

Prevent service from starting automatically at boot.

```bash
sudo systemctl disable nginx
```

**Expected output:**
```
Removed /etc/systemd/system/multi-user.target.wants/nginx.service.
```

---

## Check Service Status

Get detailed status information about a service.

```bash
systemctl status nginx
```

**Interpreting the output:**
- `Loaded:` - Path to service file and enable status
- `Active:` - Current state and how long it's been running
- `Main PID:` - Process ID
- `Tasks:` - Number of threads/tasks
- `Memory:` - Current memory usage
- `CGroup:` - Process hierarchy

---

## View Logs

Stream logs for a specific service.

```bash
journalctl -u nginx
```

**Show last 50 lines:**
```bash
journalctl -u nginx -n 50
```

**Follow logs in real-time:**
```bash
journalctl -u nginx -f
```

**Show logs since last boot:**
```bash
journalctl -u nginx -b
```

**Show logs with timestamps:**
```bash
journalctl -u nginx --no-pager -o short-iso
```

**Example output:**
```
2024-01-16T10:30:45.123Z nginx[12345]: Starting A high performance web server and a reverse proxy server...
2024-01-16T10:30:45.456Z nginx[12345]: Started A high performance web server and a reverse proxy server.
2024-01-16T10:35:20.789Z systemd[1]: Stopping A high performance web server and a reverse proxy server...
```

---

# List Services

## List all services with status

```bash
systemctl list-units --type=service
```

**Example output:**
```
UNIT                      LOAD   ACTIVE SUB     DESCRIPTION
accounts-daemon.service   loaded active running Accounts Service
acpid.service             loaded active running ACPI event daemon
atd.service               loaded active running Deferred execution scheduler
bluetooth.service         loaded active running Bluetooth service
cron.service              loaded active running Regular background program processing daemon
cups.service              loaded active running CUPS Scheduler
dbus.service              loaded active running D-Bus System Message Bus
docker.service            loaded active running Docker Application Container Engine
nginx.service             loaded active running A high performance web server and a reverse proxy server
ssh.service               loaded active running OpenBSD Secure Shell server
systemd-journald.service  loaded active running Journal Service
systemd-logind.service    loaded active running Login Service
```

## List only running services

```bash
systemctl list-units --type=service --state=running
```

## List only failed services

```bash
systemctl list-units --type=service --state=failed
```

**Troubleshooting failed services:**
```bash
systemctl status <service-name>
journalctl -u <service-name> -n 100
```

## List enabled services

```bash
systemctl list-unit-files --type=service --state=enabled
```

---

# Service Unit Files

## Understanding Service Unit File Anatomy

Service files are located in:
- `/lib/systemd/system/` (system services)
- `/etc/systemd/system/` (custom/overridden services)
- `~/.config/systemd/user/` (user services)

### Example: Complete nginx.service File

```ini
[Unit]
Description=A high performance web server and a reverse proxy server
Documentation=man:nginx(8)
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/sbin/nginx -t
ExecStart=/usr/sbin/nginx
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true
Restart=on-failure
RestartSec=10
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
```

**Section explanations:**

**[Unit]** - Describes the unit
- `Description:` - Human-readable description
- `Documentation:` - Link to documentation
- `After:` - Start this unit after these units (ordering, not dependency)
- `Wants:` - Soft dependency (unit fails if dependency fails, but not critical)
- `Requires:` - Hard dependency (unit fails if dependency fails)

**[Service]** - Service-specific settings
- `Type=forking:` - Service forks itself and exits (traditional)
  - `simple:` - Service stays in foreground (default)
  - `oneshot:` - Script-like, completes and exits
  - `dbus:` - Service acquires D-Bus name
- `PIDFile:` - Path to PID file
- `ExecStartPre:` - Command to run before start
- `ExecStart:` - Main start command
- `ExecReload:` - Reload configuration command
- `ExecStop:` - Stop command
- `Restart:` - Restart policy (always, on-failure, on-success, no)
- `RestartSec:` - Wait time before restart
- `StandardOutput:` - Where to send stdout (journal, syslog, kmsg, null)
- `StandardError:` - Where to send stderr

**[Install]** - Installation settings
- `WantedBy:` - Which target to enable this service in

---

## Creating Custom Service File

Create a service file for a custom application:

```bash
sudo tee /etc/systemd/system/myapp.service > /dev/null <<EOF
[Unit]
Description=My Custom Application
After=network.target

[Service]
Type=simple
User=myapp
WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/bin/myapp
Restart=on-failure
RestartSec=10
StandardOutput=journal
StandardError=journal
Environment="LOG_LEVEL=info"

[Install]
WantedBy=multi-user.target
EOF
```

**Enable and start the service:**
```bash
sudo systemctl daemon-reload  # Reload systemd configuration
sudo systemctl enable myapp
sudo systemctl start myapp
sudo systemctl status myapp
```

---

# Timer Units (systemd Timers as Alternative to Cron)

systemd timers provide a modern replacement for cron with better logging, dependencies, and systemd integration.

## Timer Syntax

Create a matching `.timer` file:

```bash
sudo tee /etc/systemd/system/backup.timer > /dev/null <<EOF
[Unit]
Description=Daily backup timer
Requires=backup.service

[Timer]
OnCalendar=daily
OnCalendar=*-*-* 02:00:00
Persistent=true

[Install]
WantedBy=timers.target
EOF
```

**OnCalendar syntax:**
- `hourly` - Every hour at minute 0
- `daily` - Every day at 00:00
- `weekly` - Every week on Monday at 00:00
- `monthly` - First day of month at 00:00
- `*-*-* 02:00:00` - Every day at 2 AM
- `Mon..Fri *-*-* 09:00:00` - Weekdays at 9 AM
- `*-*-1,15 12:00:00` - 1st and 15th at noon

## Create Associated Service

```bash
sudo tee /etc/systemd/system/backup.service > /dev/null <<EOF
[Unit]
Description=Backup service
After=network.target

[Service]
Type=oneshot
ExecStart=/opt/backup.sh
StandardOutput=journal
StandardError=journal
EOF
```

## Enable and Start Timer

```bash
sudo systemctl daemon-reload
sudo systemctl enable backup.timer
sudo systemctl start backup.timer
```

## Monitor Timers

```bash
# List all timers
systemctl list-timers

# Example output:
# NEXT                       LEFT          LAST                       PASSED UNIT                       ACTIVATES
# Tue 2024-01-16 02:00:00 UTC 2h 15min left Mon 2024-01-15 02:00:05 UTC 21h 45min ago backup.timer                  backup.service

# Show timer status
systemctl status backup.timer

# View last execution
journalctl -u backup.service -n 20
```

---

# Dependency Management

## Understanding Dependencies

**Example service with dependencies:**

```ini
[Unit]
Description=Web Application
Requires=postgresql.service
After=postgresql.service
Wants=redis.service
Before=backup.service

[Service]
Type=simple
ExecStart=/opt/webapp/bin/start.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

**Dependency types:**
- `Requires=` - Hard dependency; fails if target fails
- `Wants=` - Soft dependency; continues if target fails
- `Before=` - Don't start until this service exits
- `After=` - Don't start until listed services are active

## Visualize Service Dependencies

```bash
# Generate dependency graph
systemd-analyze dot --order | dot -Tsvg > deps.svg

# View dependencies for a service
systemctl show -p Requires,After,Before,Wants nginx.service
```

---

# Restart Policies

Control how services restart on failure.

**Restart types:**
```ini
Restart=no          # Don't restart (default)
Restart=always      # Always restart when exited
Restart=on-success  # Restart only if exit code 0
Restart=on-failure  # Restart only on non-zero exit
Restart=on-abnormal # Restart on signal or timeout
RestartSec=10       # Wait 10 seconds before restart
StartLimitInterval=60min
StartLimitBurst=3   # Max 3 restarts in 60 minutes
```

**Example:**
```ini
[Service]
Restart=on-failure
RestartSec=5
StartLimitInterval=60
StartLimitBurst=3
StandardOutput=journal
StandardError=journal
```

---

# Resource Limits

Limit system resources used by a service.

```ini
[Service]
# Memory limit
MemoryLimit=512M

# CPU quota (20% of one CPU core)
CPUQuota=20%

# File descriptor limit
LimitNOFILE=65536

# Process limit
LimitNPROC=512

# Open file size limit
LimitFSIZE=1G

# Real-time priority
CPUSchedulingPolicy=batch
```

**Example resource-limited service:**

```bash
sudo tee /etc/systemd/system/worker.service > /dev/null <<EOF
[Unit]
Description=Background Worker Service

[Service]
Type=simple
ExecStart=/opt/worker/run.sh
Restart=on-failure

MemoryLimit=256M
CPUQuota=50%
LimitNOFILE=8192

StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target
EOF
```

---

# Boot Targets

systemd uses targets instead of traditional runlevels.

## Check Default Target

```bash
systemctl get-default
```

**Expected output:**
```
multi-user.target
```

## List Available Targets

```bash
systemctl list-units --type=target
```

**Common targets:**
- `multi-user.target` - Multi-user mode (CLI, no X11)
- `graphical.target` - Graphical mode with desktop
- `rescue.target` - Single-user rescue mode
- `emergency.target` - Emergency shell
- `reboot.target` - Reboot
- `poweroff.target` - Shutdown

## Change Default Target

```bash
sudo systemctl set-default graphical.target
```

## Switch to Target Temporarily

```bash
sudo systemctl isolate rescue.target
```

---

# Common Troubleshooting

## Service won't start

```bash
# Check service file syntax
systemd-analyze verify /etc/systemd/system/myapp.service

# View detailed error logs
journalctl -u myapp.service -n 100

# Reload daemon and retry
sudo systemctl daemon-reload
sudo systemctl restart myapp
```

## Service keeps restarting

```bash
# Check resource limits
systemctl show -p MemoryLimit,CPUQuota,RestartSec myapp

# Increase RestartSec to allow recovery time
# Edit service file and reload
sudo systemctl daemon-reload
```

## Check service file for common issues

```bash
# Syntax check
sudo systemctl -l --no-pager status myapp.service

# Full service configuration
systemctl cat myapp.service
```

---

# Best Practices

1. **Always reload after editing** - `sudo systemctl daemon-reload`
2. **Use Type=simple for modern apps** - Simpler than Type=forking
3. **Set Restart policies** - Use `on-failure` for resilience
4. **Log to journal** - `StandardOutput=journal StandardError=journal`
5. **Verify service files** - Use `systemd-analyze verify`
6. **Set resource limits** - Prevent resource exhaustion
7. **Use dependencies wisely** - Prefers `Wants=` over `Requires=` (more resilient)
8. **Document custom services** - Add Description and Documentation fields

---

# Summary

systemd provides advanced service management with features like socket activation, timer-based scheduling, resource limits, and comprehensive dependency management. It replaces traditional init scripts with a more powerful and unified system.