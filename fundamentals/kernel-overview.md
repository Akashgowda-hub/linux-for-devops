# Linux Kernel Overview

The kernel is the core component of the Linux operating system, managing all hardware resources and mediating between applications and physical devices.

---

## Kernel Responsibilities

### 1. Process Management

Managing creation, scheduling, and termination of processes.

**Check process information:**
```bash
ps aux
```

**Output (abbreviated):**
```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1  24076 10892 ?        Ss   10:30   0:02 /sbin/init
root        12  0.0  0.2  34567  8234 ?        Ss   10:31   0:01 /lib/systemd/systemd-journald
ubuntu    1234  0.1  0.3  45678 12456 pts/0    Ss   11:00   0:05 bash
```

**See process hierarchy:**
```bash
pstree
```

### 2. Memory Management

Allocating and managing RAM for processes.

**Check memory usage:**
```bash
free -h
```

**Output:**
```
              total        used        free      shared  buff/cache   available
Mem:          7.8Gi       2.3Gi       1.5Gi       123Mi       3.9Gi       5.1Gi
Swap:         2.0Gi          0B       2.0Gi
```

**View page faults:**
```bash
cat /proc/vmstat | grep fault
```

### 3. Device Drivers

Providing interface between hardware and software.

**List loaded device drivers (kernel modules):**
```bash
lsmod
```

**Output (abbreviated):**
```
Module                  Size  Used by
overlay               136192  40
xt_nat                 16384  20
xt_conntrack           16384  1 xt_nat
nf_nat_masquerade_ipv4 16384  3
nf_conntrack_ipv4      24576  3 nf_nat_masquerade_ipv4
nf_nat                 45056  7 xt_nat,nf_nat_masquerade_ipv4
```

**Check USB devices:**
```bash
lsusb
```

**See PCI devices:**
```bash
lspci
```

### 4. File System Management

Managing file storage and access.

**View mounted filesystems:**
```bash
mount | grep "^/dev"
```

**Output:**
```
/dev/sda1 on / type ext4 (rw,relatime)
/dev/sda2 on /home type ext4 (rw,relatime)
/dev/sda3 on /var type ext4 (rw,relatime)
```

**Check filesystem types:**
```bash
df -T
```

### 5. Security

Enforcing access controls and permissions.

**View Security Enhanced Linux (SELinux) status:**
```bash
sestatus  # If available
```

**Check file permissions:**
```bash
ls -la /etc/passwd
```

**Output:**
```
-rw-r--r-- 1 root root 2345 Jan 8 10:30 /etc/passwd
```

### 6. Interrupt and Exception Handling

Responding to hardware and software events.

**View interrupt statistics:**
```bash
cat /proc/interrupts | head -15
```

---

## Kernel Type

### Monolithic Kernel

Linux uses a **monolithic kernel** architecture, meaning:

- **Single address space:** All kernel code runs in same memory space
- **Fast communication:** Modules can directly call each other
- **Tightly coupled:** Changes in one component can affect others
- **Large kernel:** All functionality compiled into one binary

**Compare with alternatives:**
- **Microkernel:** Small core, services as separate processes (Minix)
- **Hybrid:** Combination of both approaches (Windows NT)

---

## Check Kernel Version

### uname - Kernel Information

**Show kernel release:**
```bash
uname -r
```

**Output:**
```
5.15.0-84-generic
```

**Breakdown:**
- `5` - Major version
- `15` - Minor version
- `0` - Patch version
- `84` - Ubuntu-specific build number
- `generic` - Build variant

**Show all system information:**
```bash
uname -a
```

**Output:**
```
Linux ubuntu 5.15.0-84-generic #93-Ubuntu SMP Fri Jan 8 10:30:45 UTC 2024 x86_64 GNU/Linux
```

**Other uname options:**
```bash
uname -m  # Hardware platform (x86_64, arm64, etc.)
uname -s  # OS name (Linux)
uname -n  # Hostname
uname -v  # Build version
```

---

## View Kernel Messages

### dmesg - Display Kernel Messages

**Show kernel boot messages:**
```bash
dmesg
```

**Output (abbreviated):**
```
[    0.000000] Linux version 5.15.0-84-generic (builder@lgw02-amd64-010) (gcc-11 (Ubuntu 11.2.0-19ubuntu1) 11.2.0, GNU ld (GNU Binutils for Ubuntu) 2.37) #93-Ubuntu SMP
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.15.0-84-generic root=/dev/mapper/vg-root
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.000000]   AMD AuthenticAMD
[    0.000000]   Hygon HygonGenuine
[    0.000000]   Centaur CentaurHauls
```

**Show recent messages:**
```bash
dmesg | tail -20
```

**Show specific messages (error):**
```bash
dmesg | grep -i error
```

**Monitor kernel messages in real-time:**
```bash
dmesg -w
```

**Show since boot:**
```bash
dmesg -b
```

**Show with timestamps:**
```bash
dmesg -T
```

---

## Kernel Modules

Kernel modules are pieces of code that can be loaded and unloaded from the kernel without rebooting.

### lsmod - List Loaded Modules

**Show all loaded modules:**
```bash
lsmod
```

**Output (abbreviated):**
```
Module                  Size  Used by
overlay               136192  40
xt_nat                 16384  20
nf_conntrack_ipv4      24576  3 nf_nat_masquerade_ipv4
nf_nat                 45056  7 xt_nat,nf_nat_masquerade_ipv4
nf_conntrack          176128  4 nf_nat,nf_conntrack_ipv4
```

**Count modules:**
```bash
lsmod | wc -l
```

**Search for specific module:**
```bash
lsmod | grep docker
```

### modprobe - Load/Unload Modules

**Load a kernel module:**
```bash
sudo modprobe module_name
```

**Example - Load RAID support:**
```bash
sudo modprobe raid1
```

**Load with parameters:**
```bash
sudo modprobe kvm max_vcpus=32
```

**Remove a kernel module:**
```bash
sudo rmmod module_name
```

**Example - Unload a module:**
```bash
sudo rmmod floppy  # Remove floppy disk support
```

**View module information:**
```bash
modinfo i915  # Intel GPU driver
```

**Load all dependent modules automatically:**
```bash
sudo modprobe --first-time nvidia  # With dependencies
```

---

## Kernel Parameters

### /proc/sys - Kernel Tuning

View and modify kernel parameters (runtime configuration).

**View parameter:**
```bash
cat /proc/sys/kernel/panic
```

**Output:**
```
0
```

**Set parameter (temporary):**
```bash
echo 10 | sudo tee /proc/sys/kernel/panic
```

**View network parameters:**
```bash
cat /proc/sys/net/ipv4/ip_forward
```

**Set network parameter (enable IP forwarding):**
```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

**View all parameters:**
```bash
sysctl -a | head -30
```

**Make changes permanent (edit /etc/sysctl.conf):**
```bash
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p  # Apply changes
```

---

## View Kernel Source Information

### /proc/version - Kernel Version File

**Check from /proc:**
```bash
cat /proc/version
```

**Output:**
```
Linux version 5.15.0-84-generic (builder@lgw02-amd64-010) (gcc-11 (Ubuntu 11.2.0-19ubuntu1) 11.2.0, GNU ld (GNU Binutils for Ubuntu) 2.37) #93-Ubuntu SMP Fri Jan 8 10:30:45 UTC 2024 x86_64 GNU/Linux
```

### /proc/cmdline - Kernel Command Line

**Check boot parameters:**
```bash
cat /proc/cmdline
```

**Output:**
```
BOOT_IMAGE=/boot/vmlinuz-5.15.0-84-generic root=/dev/mapper/vg-root ro quiet splash vt_handoff=7
```

---

## Kernel Upgrade

### Check for updates

**Ubuntu/Debian:**
```bash
apt list --upgradable | grep linux
```

**Install new kernel:**
```bash
sudo apt install linux-image-generic linux-headers-generic
```

**Reboot to use new kernel:**
```bash
sudo reboot
```

**Check current kernel after reboot:**
```bash
uname -r
```

---

## Kernel Compilation (Advanced)

For custom kernel building:

```bash
# Get kernel source
sudo apt install linux-source
cd /usr/src/linux-source-5.15.0

# Configure kernel
make menuconfig

# Compile
make -j$(nproc)

# Install
sudo make modules_install install

# Update bootloader
sudo update-initramfs -c -k all
sudo update-grub

# Reboot
sudo reboot
```

---

## Useful Kernel Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `uname -r` | Show kernel version | `5.15.0-84-generic` |
| `dmesg` | View kernel messages | Troubleshooting boot |
| `lsmod` | List loaded modules | See active drivers |
| `modprobe` | Load/unload modules | Add functionality |
| `cat /proc/version` | Detailed version info | Build details |
| `sysctl` | View/set parameters | Tune kernel behavior |

---

## Best Practices

1. **Keep kernel updated:**
   ```bash
   sudo apt update && sudo apt upgrade
   ```

2. **Check for kernel issues:**
   ```bash
   dmesg | grep -E "error|fail|warn"
   ```

3. **Monitor loaded modules:**
   ```bash
   lsmod | wc -l  # Should be reasonable number
   ```

4. **Document kernel changes:**
   ```bash
   cat /proc/cmdline  # Save boot parameters
   lsmod > ~/kernel_modules.txt
   ```

5. **Tune for workload:**
   ```bash
   sysctl -a > ~/sysctl_defaults.txt  # Backup before changes
   ```

---

## Summary

The Linux kernel is a monolithic kernel that:
- Manages processes, memory, and hardware
- Communicates with applications via system calls
- Loads/unloads modules for extensibility
- Can be tuned and optimized for specific workloads
- Is regularly updated for security and performance