# Linux System Architecture

Linux is a layered architecture where each layer provides specific functionality and abstracts the complexity of lower layers.

---

## Architecture Layers

The Linux system consists of four main layers:

```text
┌─────────────────────────────────────┐
│        User Applications            │
│    (web browsers, editors, etc.)    │
├─────────────────────────────────────┤
│        Shell / CLI Interface        │
│    (bash, zsh, terminal emulator)   │
├─────────────────────────────────────┤
│        Linux Kernel                 │
│  (process, memory, device mgmt)     │
├─────────────────────────────────────┤
│        Hardware                     │
│  (CPU, RAM, disk, network cards)    │
└─────────────────────────────────────┘
```

### Layer Details

**User Applications:**
- Web browsers, text editors, IDEs
- Database servers, web servers
- Custom applications
- These run with restricted access

**Shell & CLI:**
- Bash, Zsh, Sh - command interpreters
- Take user input and execute commands
- Communicate with kernel via system calls

**Kernel:**
- Core of the operating system
- Manages hardware resources
- Executes user programs safely
- Handles system calls

**Hardware:**
- CPU (processor)
- RAM (memory)
- Storage (disks)
- Network interfaces

---

## Hardware Components

### CPU (Processor)

**Check CPU information:**
```bash
cat /proc/cpuinfo | head -15
```

**Output:**
```
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model name      : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
cpu MHz         : 2000.000
cache size      : 8192 KB
cpuid level     : 22
wp              : yes
```

**Count cores:**
```bash
nproc
```

**Output:**
```
4
```

**Show CPU details:**
```bash
lscpu
```

### RAM (Memory)

**Check memory:**
```bash
cat /proc/meminfo | head -10
```

**Output:**
```
MemTotal:        8167852 kB
MemFree:         4234560 kB
MemAvailable:    5123456 kB
Buffers:          234567 kB
Cached:          1234567 kB
SwapTotal:       2097152 kB
SwapFree:        2097152 kB
```

**Show human-readable memory:**
```bash
free -h
```

**Output:**
```
              total        used        free      shared  buff/cache   available
Mem:          7.8Gi       2.3Gi       1.5Gi       123Mi       3.9Gi       5.1Gi
Swap:         2.0Gi          0B       2.0Gi
```

### Disk Storage

**List block devices:**
```bash
lsblk
```

**Output:**
```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0   100G  0 disk
├─sda1   8:1    0     1G  0 part /boot
├─sda2   8:2    0    99G  0 part /
└─sda3   8:3    0     2G  0 part [SWAP]
sdb      8:16   0   500G  0 disk
└─sdb1   8:17   0   500G  0 part /data
```

**Check disk usage:**
```bash
df -h
```

### Network Interface

**Show network interfaces:**
```bash
ip link show
```

**Output:**
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:a1:b2:c3 brd ff:ff:ff:ff:ff:ff
```

---

## Kernel

The kernel is the core component that manages all system resources and mediates between applications and hardware.

### Kernel Responsibilities

**Process Management:**
```bash
# List processes
ps aux

# See process tree
pstree
```

**Memory Management:**
```bash
# Check memory usage
cat /proc/self/maps  # Memory map of current process
```

**Device Management:**
```bash
# See loaded drivers
lsmod

# Check device information
dmidecode | grep "System Information" -A 5
```

**File System Management:**
```bash
# See mounted filesystems
mount | grep "^/dev"

# Check filesystem type
df -T
```

**Check Kernel Version:**
```bash
uname -r
```

**Output:**
```
5.15.0-84-generic
```

**Detailed kernel info:**
```bash
uname -a
```

**Output:**
```
Linux ubuntu 5.15.0-84-generic #93-Ubuntu SMP Fri Jan 8 10:30:45 UTC 2024 x86_64 GNU/Linux
```

---

## Shell

The shell is the intermediary between you and the kernel. It interprets your commands and communicates with the kernel.

### Common Shells

**Check current shell:**
```bash
echo $SHELL
```

**Output:**
```
/bin/bash
```

**Bash (Bourne Again Shell):**
- Most common default shell
- Feature-rich scripting
- Good balance of power and ease
- Used in most Linux systems

**Zsh (Z Shell):**
- Modern interactive shell
- Better tab completion
- Powerful globbing
- Popular in development environments

**Sh (POSIX Shell):**
- Minimal, portable shell
- Used for system scripts
- Available on all Unix systems

---

## User Space vs Kernel Space

Understanding this distinction is crucial for system security and performance.

| Aspect | User Space | Kernel Space |
|--------|------------|--------------|
| Access | Limited to own process | Full hardware access |
| Permissions | User privileges | Root/kernel privileges |
| Memory | Isolated memory segment | Full system memory |
| Operations | Restricted operations | All operations allowed |
| Crash impact | Only affects that process | Can crash entire system |
| Performance | Slightly slower (context switch) | Faster, direct access |

**Example: Accessing hardware**

```bash
# User space - cannot directly access hardware
echo "Hello" > /dev/sda  # Permission denied

# Kernel space - can access via system calls
sudo dd if=image.iso of=/dev/sda  # Works - uses syscall to kernel
```

---

## System Calls

Applications don't directly access hardware; they use system calls to ask the kernel to do things.

**Common system calls:**
```
open()   - Open a file
read()   - Read from file/device
write()  - Write to file/device
fork()   - Create new process
exec()   - Execute program
exit()   - Terminate process
kill()   - Send signal to process
socket() - Create network socket
mount()  - Mount filesystem
```

**Trace system calls:**
```bash
strace ls /tmp
```

**Output (abbreviated):**
```
execve("/bin/ls", ["ls", "/tmp"], ...) = 0
brk(NULL)                               = 0x563d5000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fbb9e2f3000
access("/etc/ld.so.preload", R_OK)     = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/tmp", O_RDONLY|O_CLOEXEC) = 3
getdents64(3, dirbuf, 32768)            = 4096
close(3)                                = 0
```

---

## Useful Commands

### uname - System Information

**Show kernel release:**
```bash
uname -r
```

**Show all system information:**
```bash
uname -a
```

**Show hardware platform:**
```bash
uname -m
```

### hostnamectl - Hostname & System Info

**Show system information:**
```bash
hostnamectl
```

**Output:**
```
   Static hostname: ubuntu
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 1234567890abcdef
           Boot ID: abcdef1234567890
    Virtualization: kvm
  Operating System: Ubuntu 22.04.1 LTS
            Kernel: Linux 5.15.0-84-generic
      Architecture: x86-64
```

**Change hostname:**
```bash
sudo hostnamectl set-hostname newname
```

### lscpu - CPU Details

**Show CPU information:**
```bash
lscpu
```

**Output (abbreviated):**
```
Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian
Address sizes:                   39 bits physical, 48 bits virtual
CPU(s):                          4
On-line CPU(s) list:             0-3
Thread(s) per core:              2
Core(s) per socket:              2
Socket(s):                       1
NUMA node(s):                    1
```

---

## System Resource Monitoring

**Check what's using resources:**
```bash
top -b -n 1 | head -20
```

**Monitor in real-time:**
```bash
htop  # If installed (sudo apt install htop)
```

**Check I/O:**
```bash
iostat
```

---

## Best Practices

1. **Understand the layering:**
   - User processes are isolated
   - System calls bridge user and kernel space
   - Kernel controls hardware access

2. **Use appropriate privilege level:**
   ```bash
   # User level - regular operations
   ls /home
   
   # Elevated - when needed
   sudo fdisk -l
   ```

3. **Monitor system resources:**
   ```bash
   free -h              # Memory
   df -h                # Disk
   top -b -n 1          # CPU and processes
   ```

4. **Understand your hardware:**
   ```bash
   lscpu                # CPU specs
   dmidecode            # Hardware details
   lsblk                # Storage layout
   ```

---

## Summary

Linux follows a layered architecture:
- **Hardware** → Physical components (CPU, RAM, disk)
- **Kernel** → Manager of hardware and resources
- **Shell** → Command interpreter
- **Applications** → User programs

Understanding this architecture helps with:
- System troubleshooting
- Performance tuning
- Security configuration
- Resource management