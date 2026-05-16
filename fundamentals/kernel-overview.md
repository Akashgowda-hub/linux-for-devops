# Linux Kernel Overview

The kernel is the core component of Linux.

---

# Kernel Responsibilities

- Process Management
- Memory Management
- Device Drivers
- File Systems
- Security

---

# Kernel Type

Linux uses a Monolithic Kernel.

---

# Check Kernel Version

```bash
uname -r
```

---

# View Kernel Messages

## dmesg

Show kernel logs.

```bash
dmesg
```

---

# Kernel Modules

## lsmod

Show loaded kernel modules.

```bash
lsmod
```

---

## modprobe

Load module.

```bash
sudo modprobe module_name
```

---

## rmmod

Remove module.

```bash
sudo rmmod module_name
```

---

# Useful Commands

## uname -a

Detailed kernel information.

```bash
uname -a
```

---

## cat /proc/version

Show Linux version.

```bash
cat /proc/version
```

---

# Summary

The kernel manages communication between software and hardware.