# Process Management

Processes are running instances of programs. Learn to monitor, manage, and control them.

---

## ps - Process Status

Display information about running processes.

```bash
# Show all processes in table format
ps -ef

# Show with tree view (parent-child relationships)
ps -efH
ps --forest

# Show processes for current user
ps

# Show detailed information
ps aux

# Filter by name
ps -ef | grep nginx

# Show only PID and command
ps -eo pid,cmd

# Show memory usage
ps aux --sort=-%mem | head

# Show CPU usage
ps aux --sort=-%cpu | head
```

**Output fields:**
- `UID` — User ID who started the process
- `PID` — Process ID
- `PPID` — Parent Process ID
- `CMD` — Command that started the process
- `%CPU` — CPU usage percentage
- `%MEM` — Memory usage percentage

**Examples:**
```bash
$ ps -ef | head -15
UID      PID PPID  C STIME TTY      TIME CMD
root       1    0  0 10:30 ?    00:00:05 /sbin/init
root       2    0  0 10:30 ?    00:00:00 [kthreadd]
www-data 1234    1  0 10:35 ?    00:00:12 nginx: master

$ ps aux | grep python
user    5678 0.5  2.3 245612 12345 ?  S 10:40  0:05 python app.py

$ ps --forest | grep ssh
    5432 ?    Ss    0:00 sshd: john [priv]
    5456 ?    S     0:00  \_ sshd: john [user]
```

---

## Searching Processes

### grep process

Search for processes by name.

```bash
# Search for specific process
ps -ef | grep nginx

# Search without grep itself in results
ps -ef | grep [n]ginx

# Search for all Python processes
ps -ef | grep python

# Count processes
ps -ef | grep -c nginx

# Show process tree
pgrep -f "process_name"
```

**Examples:**
```bash
$ ps -ef | grep nginx
root      1234     1  0 10:35 ?    00:00:00 nginx: master process
www-data  1235  1234  0 10:35 ?    00:00:02 nginx: worker process
www-data  1236  1234  0 10:35 ?    00:00:01 nginx: worker process

# Count nginx processes
$ ps -ef | grep -c nginx
3

# Find PID without matching grep
$ pgrep nginx
1234
1235
1236
```

---

## top - Real-Time Process Monitoring

Interactive system monitoring tool.

```bash
# Start top
top

# Batch mode (single snapshot)
top -b -n 1

# Monitor specific process
top -p PID

# Sort by CPU usage (default)
top -o %CPU

# Sort by memory usage
top -o %MEM

# Update every N seconds
top -d 5

# Show N iterations then exit
top -n 3
```

**Interactive commands:**
- `P` — Sort by CPU
- `M` — Sort by memory
- `T` — Sort by time
- `q` — Quit
- `k` — Kill process (enter PID)
- `r` — Renice (change priority)

**Examples:**
```bash
$ top -b -n 1 | head -20
top - 10:45:32 up 5 days,  3:15,  2 users,  load average: 0.85, 0.92, 0.88
Tasks:  187 total,   2 running, 185 sleeping,   0 stopped
%Cpu(s):  5.2 us,  2.1 sy,  0.0 ni, 92.3 id,  0.2 wa

  PID USER      PR  NI    VIRT    RES    SHR  %CPU %MEM  TIME+ COMMAND
 5678 john      20   0  245612  12345  8901 45.3  3.2  5:23 python
 1234 root      20   0   12345   6789  5432 12.1  1.8  2:15 nginx
 9012 mysql     20   0  1234567 567890 45678  5.2 15.6 10:45 mysqld
```

---

## htop - Interactive Process Viewer

More user-friendly alternative to top.

```bash
# Start htop
htop

# Monitor specific user
htop -u username

# Monitor specific PID
htop -p PID

# Show threads
htop -H

# Display in tree view
# Press 'F5' inside htop
```

**Advantages over top:**
- Color-coded CPU cores
- Horizontal/vertical scrolling
- Easier to navigate
- Better visual representation

**Examples:**
```bash
# Install htop
sudo apt install htop

# Run htop
$ htop
```

---

## kill - Terminate Process

Send signals to processes (terminate, pause, etc.).

```bash
# Terminate process gracefully (SIGTERM)
kill PID

# Force kill process (SIGKILL)
kill -9 PID

# List available signals
kill -l

# Send specific signal
kill -SIGTERM PID
kill -15 PID

# Kill multiple processes
kill PID1 PID2 PID3

# Kill by name (killall)
killall processname

# Kill by pattern (pkill)
pkill -f "pattern"
```

**Common signals:**
- `SIGTERM (15)` — Graceful termination (catchable)
- `SIGKILL (9)` — Force kill (cannot be caught)
- `SIGSTOP (19)` — Pause process
- `SIGCONT (18)` — Resume process
- `SIGHUP (1)` — Hang up (restart)

**Examples:**
```bash
# Gracefully stop nginx
$ kill 1234
$ ps -ef | grep nginx
# Process stops after cleanup

# Force kill stuck process
$ kill -9 5678

# Kill all instances of python
$ killall python

# Kill by pattern
$ pkill -f "java -jar app.jar"

# Terminate and see result
$ kill 1234 && echo "Process terminated"
Process terminated
```

---

## kill -9 - Force Kill Process

Forcefully terminate a process that won't respond to normal kill.

```bash
# Force kill
kill -9 PID

# Force kill by name
killall -9 processname

# Force kill all instances
pkill -9 -f "pattern"
```

**When to use:**
- Process is hung/frozen
- Normal kill doesn't work
- Emergency shutdown needed

**Examples:**
```bash
# Normal kill doesn't work
$ kill 5678
$ ps -ef | grep 5678
root 5678 ... # Still running

# Force kill it
$ kill -9 5678
$ ps -ef | grep 5678
# Process removed

# Kill zombie processes
$ ps aux | grep defunct
root 6789 ... <defunct>
$ kill -9 6789
```

---

## Process Monitoring Commands

### nproc - CPU Core Count

Show number of CPU cores available.

```bash
# Show total cores
nproc

# Show all including offline
nproc --all
```

**Examples:**
```bash
$ nproc
8

$ nproc --all
16
```

### free - Memory Usage

Display memory and swap usage.

```bash
# Human-readable format
free -h

# Show in MB
free -m

# Show in GB
free -g

# With total
free -h --total

# Continuous display
free -h -s 5  # Update every 5 seconds
```

**Output explanation:**
```
               total        used        free      shared  buff/cache   available
Mem:           7.8Gi       2.3Gi       1.2Gi      512Mi       4.3Gi       4.8Gi
Swap:          2.0Gi       0.0Gi       2.0Gi
```

**Examples:**
```bash
$ free -h
               total        used        free      shared  buff/cache   available
Mem:           7.8Gi       2.3Gi       1.2Gi      512Mi       4.3Gi       4.8Gi
Swap:          2.0Gi       100Mi       1.9Gi

$ free -h --total
               total        used        free      shared  buff/cache   available
Mem:           7.8Gi       2.3Gi       1.2Gi      512Mi       4.3Gi       4.8Gi
Swap:          2.0Gi       100Mi       1.9Gi
Total:        9.8Gi       2.4Gi       3.1Gi
```

### uptime - System Uptime

Show how long system has been running.

```bash
# Show uptime
uptime

# Just load average
uptime | awk -F'load average:' '{print $2}'
```

**Output explanation:**
```
 10:45:32          - Current time
 up 5 days,        - Uptime
 3 users,          - Logged in users
 load average: 0.85, 0.92, 0.88  - Load (1min, 5min, 15min)
```

**Examples:**
```bash
$ uptime
 10:45:32 up 5 days,  3:15,  3 users,  load average: 0.85, 0.92, 0.88

# If CPU has 4 cores:
# load 0.85 = 21% utilized
# load 2.00 = 50% utilized
# load 4.00 = 100% utilized (saturated)

$ uptime -p
up 5 days, 3 hours, 15 minutes
```

---

## Process Priority Management

### nice - Run with Different Priority

Run process with different scheduling priority.

```bash
# Run with lower priority (higher nice value = lower priority)
nice -n 10 command

# Run with higher priority (requires sudo, negative value)
sudo nice -n -10 command

# Check default nice value
nice
```

### renice - Change Running Process Priority

Change priority of already running process.

```bash
# Increase nice (lower priority)
renice -n 5 -p PID

# Decrease nice (higher priority, requires sudo)
sudo renice -n -5 -p PID

# Change multiple processes
renice -n 10 -p PID1 PID2 PID3
```

**Examples:**
```bash
$ nice -n 10 heavy_computation.sh
$ ps -eo pid,ni,cmd | grep heavy
12345 10 heavy_computation.sh

$ sudo renice -n -5 -p 12345
-p 12345: old priority 10, new priority -5
```

---

## Summary

Process management is essential for system administration. Monitor processes regularly, terminate cleanly when possible, and use force kill only when necessary.