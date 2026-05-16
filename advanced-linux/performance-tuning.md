# Performance Tuning

Performance tuning optimizes Linux systems for speed, reliability, and efficiency. It involves monitoring, analysis, and targeted improvements.

---

# Performance Tuning Methodology

```
1. MONITOR → Collect performance metrics
2. ANALYZE → Identify bottlenecks
3. PROFILE → Understand root causes
4. OPTIMIZE → Apply targeted fixes
5. VALIDATE → Measure improvements
6. REPEAT → Continuous improvement
```

---

# CPU Performance Monitoring

## top - Real-time Process Monitoring

```bash
top
```

**Key metrics:**
- `%CPU` — CPU usage by process
- `%MEM` — Memory usage
- `VIRT` — Virtual memory
- `RES` — Physical memory

**Interactive commands:**
- `P` — Sort by CPU
- `M` — Sort by memory
- `q` — Quit

**Example:**

```bash
top -b -n 1 | head -20  # Single output, batch mode
```

## htop - Interactive Process Monitor

```bash
htop
```

More user-friendly than `top`, shows:
- Color-coded CPU cores
- Memory usage bars
- Easy sorting and filtering

## Load Average

```bash
uptime
# Output: 12:30:45 up 5 days, 3:10, 2 users, load average: 0.85, 0.92, 0.88

# Load average interpretation:
# 0.85 (1-min), 0.92 (5-min), 0.88 (15-min)
# If load > CPU cores, system is overloaded
```

## CPU Usage Analysis

```bash
# Per-CPU breakdown
mpstat -P ALL 1 3

# Show CPU affinity
taskset -p -c <PID>

# List CPU cores
nproc
lscpu | grep "CPU(s)"
```

---

# Memory Performance Monitoring

## free - Memory Overview

```bash
free -h
# Output example:
#               total        used        free      shared  buff/cache   available
# Mem:           7.8Gi       2.3Gi       1.2Gi      512Mi       4.3Gi       4.8Gi
# Swap:          2.0Gi       0.0Gi       2.0Gi
```

**Interpretation:**
- `available` — Memory available for new processes (includes cache that can be freed)
- `buff/cache` — OS cache, can be freed if needed

## vmstat - Virtual Memory Statistics

```bash
vmstat 1 5  # Display 5 samples, 1 second interval
```

**Key columns:**
- `r` — Runnable processes (CPU queue)
- `b` — Blocked processes (waiting for I/O)
- `wa` — CPU wait time for I/O
- `si/so` — Swap in/out (page faults)

**High wa% or swap activity indicates memory pressure.**

## Memory Leak Detection

```bash
# Monitor process memory growth
while true; do ps aux | grep processname | grep -v grep | awk '{print $6}'; sleep 5; done

# Use valgrind for detailed analysis
valgrind --leak-check=full ./myapp
```

## Memory Pressure Tuning

```bash
# Check swappiness (0-100, lower = less swap)
cat /proc/sys/vm/swappiness

# Reduce swap usage (temporary)
sysctl vm.swappiness=10

# Make permanent
echo "vm.swappiness=10" >> /etc/sysctl.conf
sysctl -p
```

---

# Disk I/O Performance Monitoring

## iostat - I/O Statistics

```bash
iostat -x 1 3  # Extended stats, 1s interval, 3 samples
```

**Key metrics:**
- `r/s, w/s` — Read/write operations per second
- `rKB/s, wKB/s` — Read/write throughput
- `%util` — Percentage disk utilization (100% = saturated)
- `await` — Average wait time (milliseconds)

**High %util or await indicates I/O bottleneck.**

## Disk Space Usage

```bash
# Human-readable disk usage
df -h

# Sort by usage
df -h | sort -k5 -rn

# Directory size
du -sh /var/log
du -sh ./* | sort -h

# Find large files
find / -type f -size +100M -exec ls -lh {} \; | head -20
```

## Disk I/O Analysis

```bash
# Monitor I/O in real-time
iotop -o

# Show I/O by process
iotop

# Check disk scheduling
cat /sys/block/sda/queue/scheduler

# Change I/O scheduler
echo 'deadline' > /sys/block/sda/queue/scheduler
```

---

# Network Performance Monitoring

## Network Interface Stats

```bash
# Real-time network interface monitoring
watch -n 1 'cat /proc/net/dev'

# Using ss (socket statistics)
ss -s                    # Summary
ss -tuln                 # Listening TCP/UDP sockets
ss -tpan | grep ESTABLISHED  # All connections

# Using netstat
netstat -tuln           # Listening ports
netstat -s              # Network statistics
```

## Network Throughput

```bash
# Install iftop for bandwidth monitoring
sudo apt install iftop

# Monitor top talkers
iftop -n

# Specific interface
iftop -i eth0
```

## Packet Loss & Latency

```bash
# Ping with statistics
ping -c 10 -s 1024 example.com

# Traceroute
traceroute example.com

# Show network delays
tc qdisc show dev eth0
```

---

# System Resource Limits

## File Descriptors

```bash
# Check limits
ulimit -a

# Show open files limit
ulimit -n

# Increase file descriptors
ulimit -n 65536

# Make permanent
echo "* soft nofile 65536" >> /etc/security/limits.conf
echo "* hard nofile 65536" >> /etc/security/limits.conf
```

## Process Limits

```bash
# Check current limits
cat /proc/sys/kernel/pid_max
cat /proc/sys/kernel/threads-max

# Increase PID max
sysctl kernel.pid_max=4194304
```

## Open Files Monitoring

```bash
# List open files by process
lsof -p <PID>

# Count open files per user
lsof | awk '{print $1}' | sort | uniq -c | sort -rn

# Find process with most open files
lsof -a | awk '{print $1}' | sort | uniq -c | sort -rn | head -5
```

---

# Process Priority Management

## Process Niceness

```bash
# Check process priority
ps -eo pid,ni,comm

# Run command with lower priority (higher nice value = lower priority)
nice -n 10 ./heavy-task.sh

# Change running process priority
renice -n 5 -p <PID>

# Run with high priority (requires sudo)
nice -n -10 ./critical-task.sh
```

## CPU Affinity

```bash
# Run process on specific CPUs
taskset -c 0,1 ./myapp

# Pin existing process to CPUs
taskset -pc 0,1 <PID>

# Check affinity
taskset -pc <PID>
```

---

# System Tuning Parameters

## kernel.sched Parameters

```bash
# Schedule check frequency
sysctl kernel.sched_latency_ns

# Minimum time slice
sysctl kernel.sched_min_granularity_ns

# Reduce context switches (increase for high-load systems)
echo "kernel.sched_migration_cost_ns=5000000" >> /etc/sysctl.conf
```

## TCP/Network Tuning

```bash
# Increase TCP backlog
sysctl net.core.somaxconn=4096

# Increase file descriptor limit
sysctl fs.file-max=2097152

# Enable TCP fast open
sysctl net.ipv4.tcp_fastopen=3

# Optimize for high throughput
sysctl net.ipv4.tcp_tw_reuse=1
sysctl net.ipv4.tcp_timestamps=1
```

---

# Practical Performance Tuning Examples

## Example 1: Identify and Fix Slow Application

```bash
#!/bin/bash
# Performance diagnosis script

echo "=== Performance Diagnosis ==="

# 1. High CPU processes
echo ""
echo "Top CPU consumers:"
ps aux --sort=-%cpu | head -6

# 2. High memory processes
echo ""
echo "Top memory consumers:"
ps aux --sort=-%mem | head -6

# 3. I/O statistics
echo ""
echo "Disk I/O status:"
iostat -x 1 2 | tail -10

# 4. Memory pressure
echo ""
echo "Memory status:"
free -h
vmstat 1 2 | tail -3

# 5. Load average
echo ""
echo "System load:"
uptime

# 6. Check for high wait time
echo ""
echo "Top processes by wait time:"
ps -eo pid,cmd,etime,wchan | head -20
```

## Example 2: Optimize Database Server

```bash
# /etc/sysctl.conf - Database server tuning

# Memory
vm.swappiness=10                    # Prefer memory over swap
vm.dirty_ratio=15                   # Flush dirty pages at 15%
vm.dirty_background_ratio=5         # Background flush at 5%

# Network (for database connections)
net.core.somaxconn=4096             # Listen queue size
net.ipv4.tcp_max_syn_backlog=4096   # SYN backlog
net.ipv4.tcp_tw_reuse=1             # Reuse TIME_WAIT sockets

# File descriptors
fs.file-max=2097152

# IPC (for shared memory)
kernel.shmmax=137438953472          # Max shared memory segment
kernel.shmall=33554432              # Total shared memory pages

# Apply settings
sysctl -p
```

## Example 3: Monitor Production System

```bash
#!/bin/bash
# Continuous monitoring script

LOG_FILE="/var/log/system-monitor.log"

while true; do
  {
    echo "=== $(date) ==="
    echo "Load: $(uptime | awk -F'load average:' '{print $2}')"
    echo "Memory: $(free -h | grep Mem | awk '{print $3}/$2}')"
    echo "Disk: $(df -h / | awk 'NR==2 {print $5}')"
    echo "CPU Wait: $(iostat 1 1 | tail -1 | awk '{print $NF}')%"
    echo ""
  } >> $LOG_FILE
  
  sleep 300  # Every 5 minutes
done
```

---

# Performance Tuning Checklist

- [ ] **CPU** — Check for runnable queue, reduce context switches
- [ ] **Memory** — Monitor for swap usage, adjust swappiness
- [ ] **Disk I/O** — Analyze await time, check for hot disks
- [ ] **Network** — Monitor connections, check for packet loss
- [ ] **Processes** — Identify runaway processes, set priorities
- [ ] **System Limits** — Increase file descriptors, adjust kernel params
- [ ] **Application** — Profile code, optimize algorithms
- [ ] **Cache** — Utilize buffer cache, reduce I/O operations
- [ ] **Load Balancing** — Distribute load across resources
- [ ] **Monitoring** — Set up continuous monitoring and alerting

---

# Best Practices

1. **Measure first** — Establish baseline before optimization
2. **Change one thing at a time** — Isolate impact of each change
3. **Document changes** — Track what you changed and why
4. **Test in staging** — Never tune production directly
5. **Monitor continuously** — Watch for performance regressions
6. **Expect diminishing returns** — 20% effort yields 80% improvement
7. **Use profilers** — For application-level optimization
8. **Set alerting** — Notify on performance degradation

---

# Summary

Effective performance tuning requires understanding system bottlenecks through monitoring and analysis, then applying targeted optimizations. Use tools like `top`, `vmstat`, `iostat`, and `lsof` to diagnose issues, then implement fixes systematically.