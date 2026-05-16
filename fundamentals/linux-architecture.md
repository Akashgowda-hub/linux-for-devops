# Linux System Architecture

Linux architecture consists of multiple layers working together.

---

# Architecture Flow

```text
Applications
    ↓
Shell
    ↓
Kernel
    ↓
Hardware
```

---

# Hardware

Physical components:

- CPU
- RAM
- Disk
- Network Card

---

# Kernel

Core part of Linux.

Responsibilities:

- Process Management
- Memory Management
- Device Management
- File Systems

Check kernel version:

```bash
uname -r
```

---

# Shell

Interface between user and kernel.

Common shells:

- bash
- sh
- zsh

Check current shell:

```bash
echo $SHELL
```

---

# User Space vs Kernel Space

| User Space | Kernel Space |
|------------|--------------|
| Applications run here | Kernel operations |
| Limited access | Full hardware access |

---

# System Calls

Applications communicate with kernel using system calls.

Examples:

- open()
- read()
- write()

---

# Useful Commands

## uname

Show system information.

```bash
uname -a
```

---

## hostnamectl

System information.

```bash
hostnamectl
```

---

# Summary

Linux architecture follows:

User → Shell → Kernel → Hardware