# Linux Boot Process

Linux follows multiple stages during boot.

---

# Boot Process Flow

```text
BIOS/UEFI
   ↓
GRUB
   ↓
Kernel
   ↓
systemd/init
   ↓
Services
   ↓
Login
```

---

# BIOS/UEFI

- Performs hardware checks
- Starts bootloader

---

# GRUB

GRUB loads the Linux kernel.

GRUB location:

```bash
/boot/grub
```

---

# Kernel Initialization

Kernel:

- Detects hardware
- Mounts filesystem
- Starts init process

---

# systemd/init

Starts system services.

Check init process:

```bash
ps -p 1
```

---

# System Services

Examples:

- ssh
- docker
- nginx

---

# Useful Commands

## systemctl

Manage services.

```bash
systemctl status nginx
```

---

## journalctl -b

View boot logs.

```bash
journalctl -b
```

---

# Summary

Linux boot process initializes hardware and starts system services.