# Linux Boot Process

The Linux boot process consists of multiple stages that initialize hardware, load the kernel, and start system services. Understanding this process is crucial for troubleshooting boot issues.

---

## Complete Boot Process Flow

```text
┌──────────────────────────────────────────┐
│  1. BIOS/UEFI Power-On Self Test (POST)  │
│     └─ Detects hardware, runs diagnostics│
├──────────────────────────────────────────┤
│  2. Bootloader (GRUB)                    │
│     └─ Loads kernel into memory          │
├──────────────────────────────────────────┤
│  3. Kernel Initialization                │
│     └─ Detects hardware, mounts root FS  │
├──────────────────────────────────────────┤
│  4. Init System (systemd/init)           │
│     └─ Starts system services            │
├──────────────────────────────────────────┤
│  5. Runlevel/Target Reached              │
│     └─ System ready for user login       │
└──────────────────────────────────────────┘
```

---

## Stage 1: BIOS/UEFI

The first code that runs when you power on the system.

### What Happens

1. **POST (Power-On Self Test)** - Verifies hardware integrity
2. **Initializes hardware** - Detects RAM, disks, etc.
3. **Finds bootloader** - Looks for boot code on disk/network
4. **Passes control** - Loads bootloader into memory

### Check BIOS/UEFI Information

**Detect if using UEFI or BIOS:**
```bash
[ -d /sys/firmware/efi ] && echo "UEFI" || echo "BIOS"
```

**View BIOS/UEFI details:**
```bash
sudo dmidecode -t system
```

**Output:**
```
# dmidecode 3.3
Getting System Data from sysfs.
System Information
        Manufacturer: QEMU
        Product Name: Standard PC (Q35 + ICH9, 2009)
        Serial Number: Not Specified
        UUID: 12345678-1234-1234-1234-123456789abc
        Wake-up Type: Power Switch
        SKU Number: Not Specified
        Family: Not Specified
```

### Troubleshooting BIOS Boot

```bash
# Check boot order
sudo dmidecode | grep -i boot

# View BIOS settings (if supported)
sudo systemctl reboot --firmware-setup  # May enter BIOS next boot
```

---

## Stage 2: Bootloader (GRUB)

GRUB (Grand Unified Bootloader) loads the kernel into memory and passes control to it.

### GRUB Location

```bash
# GRUB configuration
ls -la /boot/grub
```

**Output:**
```
drwxr-xr-x  4 root root   4096 Jan  8 10:30 .
drwxr-xr-x  3 root root   4096 Jan  8 10:00 ..
-rw-r--r--  1 root root   1234 Jan  8 10:30 grub.cfg
-rw-r--r--  1 root root   5678 Jan  8 10:15 grubenv
drwxr-xr-x  2 root root   4096 Jan  8 09:30 fonts
drwxr-xr-x  2 root root   4096 Jan  8 09:30 i386-pc
drwxr-xr-x  2 root root   4096 Jan  8 09:30 locale
```

### Check GRUB Configuration

**View GRUB menu options:**
```bash
cat /boot/grub/grub.cfg | grep "menuentry" | head -5
```

**Output:**
```
menuentry 'Ubuntu' {
menuentry 'Ubuntu, with Linux 5.15.0-84-generic' {
menuentry 'Ubuntu, with Linux 5.15.0-84-generic (recovery mode)' {
menuentry 'Windows Boot Manager (on /dev/sda2)' --class windows --class os {
```

### GRUB Parameters

**Edit GRUB at boot time (during GRUB menu):**
1. Press `e` at GRUB menu
2. Edit kernel parameters
3. Press `Ctrl+X` to boot

**Make permanent changes:**
```bash
# Edit GRUB configuration
sudo nano /etc/default/grub

# Regenerate GRUB config
sudo update-grub

# Reboot to apply
sudo reboot
```

**Example: Add boot parameter for debugging**
```bash
# Edit default parameters
sudo nano /etc/default/grub

# Change:
# GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
# To:
# GRUB_CMDLINE_LINUX_DEFAULT="debug"

sudo update-grub
sudo reboot
```

---

## Stage 3: Kernel Initialization

The kernel loads and performs hardware detection and initialization.

### What Happens

1. **Unpacks itself** - Kernel image is compressed
2. **Initializes CPU** - Detects processor capabilities
3. **Sets up memory** - Configures virtual memory
4. **Loads drivers** - Loads essential device drivers
5. **Detects hardware** - PCI, USB, etc.
6. **Mounts root filesystem** - Loads FS from disk

### View Kernel Boot Messages

**Show all boot messages:**
```bash
dmesg
```

**Output (abbreviated):**
```
[    0.000000] Linux version 5.15.0-84-generic (builder@lgw02-amd64-010)
[    0.000000] Command line: BOOT_IMAGE=/boot/vmlinuz-5.15.0-84-generic
[    0.000000] KERNEL supported cpus:
[    0.000000]   Intel GenuineIntel
[    0.123456] clocksource: refined-jiffies: 1 Hz
[    0.234567] setup: Detected 8192 MB HIGHMEM
[    0.345678] e820: host address space size is 40 bits
```

**Show recent boot messages:**
```bash
dmesg | tail -30
```

**Check for boot errors:**
```bash
dmesg | grep -i "error\|fail\|warn" | head -10
```

**Check driver loading:**
```bash
dmesg | grep -i "driver\|module"
```

### Check Kernel Parameters

**View kernel command line:**
```bash
cat /proc/cmdline
```

**Output:**
```
BOOT_IMAGE=/boot/vmlinuz-5.15.0-84-generic root=/dev/mapper/vg-root ro quiet splash
```

---

## Stage 4: Init System (systemd)

The init system starts system services and brings the system to the target runlevel.

### Check Init System

**See which init system is running:**
```bash
ps -p 1
```

**Output:**
```
    PID TTY      STAT   TIME COMMAND
      1 ?        Ss     0:02 /lib/systemd/systemd --system --deserialize 17
```

**Modern systems use systemd**

### systemd Targets

**View available targets:**
```bash
systemctl list-units --type=target
```

**Output (abbreviated):**
```
UNIT                  LOAD   ACTIVE SUB    DESCRIPTION
basic.target          loaded active active Basic System
cryptsetup.target     loaded active active Encrypted Volumes
getty.target          loaded active active Login Prompts
graphical.target      loaded active active Graphical Interface
local-fs.target       loaded active active Local File Systems
multi-user.target     loaded active active Multi-User System
network-online.target loaded active active Network is Online
```

**Check default target:**
```bash
systemctl get-default
```

**Output:**
```
multi-user.target
```

**Common targets:**

| Target | Purpose |
|--------|---------|
| multi-user.target | Console mode (no GUI) |
| graphical.target | GUI mode |
| rescue.target | Single-user recovery mode |
| emergency.target | Minimal mode for emergencies |

**Change default target:**
```bash
sudo systemctl set-default graphical.target
sudo reboot
```

### Boot Sequence

**View what started during boot:**
```bash
systemctl list-units --type=service | head -20
```

**View service start times:**
```bash
systemd-analyze
```

**Output:**
```
Startup finished in 2.345s (kernel) + 3.456s (initrd) + 8.234s (userspace) = 14.035s
```

**See slowest services:**
```bash
systemd-analyze blame | head -10
```

**Output:**
```
         8234ms docker.service
         5678ms mysql.service
         3456ms ssh.service
```

---

## Stage 5: Runlevel/Target Reached

System is ready for user login.

### Check Current Target

**See current system state:**
```bash
systemctl status
```

**Output:**
```
   System uptime: 2 days 3 hours 45 minutes
   Failed units: 0
```

**List all running services:**
```bash
systemctl list-units --type=service --state=running
```

---

## Boot Process Troubleshooting

### Common Boot Issues

#### Stuck at GRUB

**Solution - Edit and boot:**
```bash
# At GRUB prompt, press 'e' to edit
# Review kernel parameters
# Press Ctrl+X to boot
```

#### Kernel Panic

**View error:**
```bash
dmesg | grep -i "panic\|BUG\|oops"
```

**Boot into recovery mode:**
1. Reboot and press `Shift` during GRUB
2. Select recovery kernel option
3. Choose "Drop to root shell"

#### Filesystem Won't Mount

**Check filesystem:**
```bash
sudo fsck -n /dev/sda1  # Read-only check (no changes)
sudo fsck /dev/sda1     # Repair
```

### Monitor Boot Process

**Watch systemd boot progress:**
```bash
sudo journalctl -b
```

**Show last boot logs:**
```bash
sudo journalctl -b -1  # Previous boot
```

**Check service dependencies:**
```bash
systemctl list-dependencies multi-user.target
```

---

## Boot Parameters

### Common Kernel Parameters

```bash
# View current parameters
cat /proc/cmdline

# Common parameters:
# ro              - Mount root read-only
# rw              - Mount root read-write
# quiet           - Suppress kernel messages
# splash          - Show splash screen
# init=/bin/bash  - Boot to root shell (emergency)
# systemd.unit=   - Start specific target
```

### Boot into Recovery Mode

**Method 1: During boot**
1. Press `Shift` during GRUB menu
2. Select recovery option
3. Select "Drop to root shell"

**Method 2: Edit GRUB**
1. Press `e` at GRUB menu
2. Find `linux` line
3. Add `single` at end
4. Press `Ctrl+X`

---

## Useful Boot Commands

| Command | Purpose |
|---------|---------|
| `dmesg` | View boot messages |
| `systemd-analyze` | Show boot timing |
| `journalctl -b` | Journal from current boot |
| `systemctl get-default` | Show default target |
| `ps -p 1` | Check init system |

---

## Boot Process Summary

1. **BIOS/UEFI** - Hardware detection
2. **GRUB** - Bootloader loads kernel
3. **Kernel** - Initializes hardware and mounts root FS
4. **systemd** - Starts system services
5. **Target reached** - System ready

Understanding each stage helps diagnose boot problems and optimize startup time.