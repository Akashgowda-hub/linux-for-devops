# Linux File System Structure

Linux uses a hierarchical file system starting from `/` (root). Understanding the directory structure is essential for system administration and troubleshooting.

---

## Root Directory Structure

The filesystem hierarchy follows the Filesystem Hierarchy Standard (FHS):

```text
/
├── bin          → Essential user commands
├── boot         → Kernel and bootloader files
├── dev          → Device files
├── etc          → Configuration files
├── home         → User home directories
├── lib          → Libraries required by programs
├── media        → Mount points for removable media
├── mnt          → Temporary mount points
├── opt          → Optional software
├── proc         → Virtual filesystem with process/kernel info
├── root         → Root user's home directory
├── run          → Runtime data (PIDs, sockets)
├── sbin         → System administration commands
├── tmp          → Temporary files (cleared on reboot)
├── usr          → User applications and utilities
└── var          → Variable data (logs, databases, mail)
```

---

## Important Directories

### /bin - Essential User Commands

Core commands available to all users.

**View contents:**
```bash
ls /bin | head -20
```

**Output:**
```
bash
cat
chmod
cp
date
echo
grep
ln
ls
mkdir
mv
ps
pwd
rm
rmdir
sed
sh
touch
wc
```

**Common examples:**
```bash
# These commands are in /bin
cat /etc/passwd
cp file.txt backup.txt
ls -la /home
```

### /sbin - System Binary (Admin Commands)

System administration commands, typically require root/sudo.

**View system commands:**
```bash
ls /sbin | head -15
```

**Output:**
```
arp
badblocks
blkid
blockdev
chkconfig
fdisk
fsck
halt
hdparm
ifconfig
ifdown
ifup
init
iptables
```

**Examples requiring sudo:**
```bash
sudo fdisk -l           # List disk partitions
sudo reboot             # Restart system
sudo shutdown -h now    # Shutdown
```

### /etc - Configuration Files

System and application configuration files. Always readable, usually need sudo to modify.

**Key configuration files:**
```bash
/etc/passwd       # User account information
/etc/shadow       # Encrypted passwords (root only)
/etc/group        # Group information
/etc/hosts        # Hostname to IP mappings
/etc/fstab        # Filesystem mount info
/etc/sudoers      # Sudo permissions
/etc/ssh/sshd_config  # SSH server config
/etc/nginx/nginx.conf # Nginx config
```

**Viewing configurations:**
```bash
# View user accounts
cat /etc/passwd | head -5
```

**Output:**
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
```

**Edit configuration (example):**
```bash
sudo nano /etc/ssh/sshd_config  # SSH configuration
sudo systemctl restart ssh      # Apply changes
```

### /home - User Home Directories

Each user has a home directory here.

**View home directories:**
```bash
ls -la /home
```

**Output:**
```
drwxr-xr-x  4 root   root   4096 Jan  8 10:00 .
drwxr-xr-x 12 root   root   4096 Jan  1 08:30 ..
drwxr-xr-x  5 ubuntu ubuntu 4096 Jan  8 11:45 ubuntu
drwxr-xr-x  3 devops devops 4096 Jan  7 09:20 devops
```

**Your home directory:**
```bash
cd ~              # Go to your home
pwd               # Show current path
# Output: /home/ubuntu

ls -la            # See your files
echo $HOME        # Echo home variable
```

### /root - Root User's Home

The root user's home directory (not in /home).

```bash
sudo ls -la /root  # View root's home (requires sudo)
sudo ls -la /root/.ssh  # Root's SSH keys
```

### /var - Variable Data

Logs, databases, caches, and other frequently changing data.

**Important subdirectories:**
```bash
/var/log      # System and application logs
/var/cache    # Cache files
/var/tmp      # Temporary files (may persist)
/var/spool    # Print queues, mail
/var/run      # Runtime info (deprecated, use /run)
```

**View logs:**
```bash
ls -la /var/log | head -15
```

**Output:**
```
-rw-r--r--  1 root root 2345678 Jan  8 11:45 syslog
-rw-r--r--  1 root root  123456 Jan  8 11:40 auth.log
-rw-r--r--  1 root root  456789 Jan  8 11:35 kern.log
drwxr-xr-x  2 root root    4096 Jan  8 10:00 nginx
drwxr-xr-x  2 root root    4096 Jan  8 09:30 mysql
```

**Check specific logs:**
```bash
tail -f /var/log/syslog        # System messages
tail -f /var/log/auth.log      # Authentication attempts
tail -f /var/log/nginx/error.log  # Web server errors
```

### /tmp - Temporary Files

Temporary storage, automatically cleaned on reboot (often via cron jobs).

**Working with temp files:**
```bash
touch /tmp/myfile.txt
echo "test data" > /tmp/data.txt
cat /tmp/data.txt
```

**Check size:**
```bash
du -sh /tmp
```

### /boot - Boot Files

Kernel, bootloader, and boot configuration.

**View contents:**
```bash
ls -la /boot
```

**Output:**
```
drwxr-xr-x  4 root root   4096 Jan  8 10:00 .
drwxr-xr-x 12 root root   4096 Jan  1 08:30 ..
-rw-r--r--  1 root root 265780 Dec 15 08:30 System.map-5.15.0-84-generic
-rw-r--r--  1 root root 217826 Dec 15 08:30 config-5.15.0-84-generic
drwxr-xr-x  2 root root   4096 Jan  8 09:00 grub
-rw-r--r--  1 root root 65381376 Dec 15 08:35 vmlinuz-5.15.0-84-generic
-rw-r--r--  1 root root 47382016 Jan  8 10:30 initrd.img-5.15.0-84-generic
```

### /dev - Device Files

Device files representing hardware and pseudo-devices.

**Common device files:**
```bash
/dev/sda        # First hard drive
/dev/sda1       # First partition
/dev/sdb        # Second hard drive
/dev/null       # Discards all data written to it
/dev/zero       # Provides null bytes
/dev/urandom    # Random number generator
/dev/tty        # Terminal
```

**Examples:**
```bash
# List block devices
lsblk

# Redirect unwanted output
command > /dev/null 2>&1

# Generate random data
dd if=/dev/urandom of=random_data bs=1M count=10
```

### /proc - Virtual Filesystem

Virtual filesystem providing information about processes and kernel.

**Explore /proc:**
```bash
# CPU information
cat /proc/cpuinfo | head -20

# Memory information
cat /proc/meminfo

# Kernel command line
cat /proc/cmdline

# Process information (PID 1234)
cat /proc/1234/status
```

**Output example (cpuinfo):**
```
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 142
model name      : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
stepping        : 10
microcode       : 0xb4
cpu MHz         : 2000.000
cache size      : 8192 KB
```

### /usr - User Applications

Non-essential programs and utilities. Contains most user-installed software.

**Key subdirectories:**
```bash
/usr/bin        # User programs
/usr/sbin       # System programs (not essential)
/usr/local      # User-compiled software
/usr/share      # Documentation and data files
/usr/lib        # Libraries for programs
```

**Examples:**
```bash
ls /usr/bin | grep -E "python|node|java"  # Check installed interpreters
ls /usr/local/bin  # Custom/compiled programs
```

---

## Useful Commands for File System Navigation

### pwd - Print Working Directory

**Show current directory:**
```bash
pwd
```

**Output:**
```
/home/ubuntu/projects
```

**Track directory depth:**
```bash
cd /var/log/nginx
pwd
# Output: /var/log/nginx
```

### df -h - Disk Usage Overview

**Show disk space (human-readable):**
```bash
df -h
```

**Output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1       100G   45G   55G  45% /
/dev/sdb1       500G  120G  380G  24% /data
tmpfs           7.8G     0  7.8G   0% /dev/shm
```

**Check specific mount:**
```bash
df -h /home
```

**Show inode usage:**
```bash
df -i /
```

### du -sh - Directory Size

**Show directory size (human-readable):**
```bash
du -sh /var/log
```

**Output:**
```
2.3G    /var/log
```

**Show sizes of subdirectories:**
```bash
du -sh /var/log/* | sort -h | tail -10
```

**Output:**
```
12M     /var/log/apt
23M     /var/log/nginx
145M    /var/log/mysql
```

**Find largest directories:**
```bash
du -sh /* | sort -rh | head -10
```

---

## File System Hierarchy - Quick Reference

| Directory | Purpose | Examples |
|-----------|---------|----------|
| /bin | Essential user commands | ls, cat, cp, grep |
| /sbin | System admin commands | fdisk, reboot, halt |
| /etc | Configuration files | passwd, hosts, ssh config |
| /home | User home directories | /home/ubuntu |
| /root | Root's home | /root |
| /var | Variable data, logs | /var/log, /var/cache |
| /tmp | Temporary files | Session files, temporary data |
| /boot | Kernel and bootloader | vmlinuz, initrd |
| /dev | Device files | /dev/sda, /dev/null |
| /proc | Process/kernel info | Process details, CPU info |
| /usr | User applications | /usr/bin, /usr/local |
| /opt | Optional software | Third-party applications |

---

## Best Practices

1. **Know where to find logs:**
   ```bash
   /var/log/syslog       # System messages
   /var/log/auth.log     # Authentication
   /var/log/*/error.log  # Application errors
   ```

2. **Back up configuration files:**
   ```bash
   sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup
   ```

3. **Monitor disk space:**
   ```bash
   df -h  # Regular check
   du -sh / | head -n 1  # Total usage
   ```

4. **Never fill /var and / - they can break the system:**
   ```bash
   df -h | grep -E "9[5-9]%|100%"  # Check for full filesystems
   ```

5. **Understanding symlinks:**
   ```bash
   ls -la /bin  # Many are symlinks to /usr/bin
   ls -l /etc/rc* | head  # Configuration symlinks
   ```

---

## Summary

Everything in Linux is treated as a file, and understanding the filesystem structure is crucial for:
- System administration
- Troubleshooting issues
- Managing storage and resources
- Finding configuration and log files