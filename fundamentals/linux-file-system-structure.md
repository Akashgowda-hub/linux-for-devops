# Linux File System Structure

Linux uses a hierarchical file system starting from `/`.

---

# Root Directory Structure

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

---

# Important Directories

## /bin

Basic user commands.

```bash
ls
cp
mv
```

---

## /sbin

System administration commands.

```bash
reboot
fdisk
```

---

## /etc

Configuration files.

Examples:

```bash
/etc/passwd
/etc/hosts
```

---

## /home

User home directories.

Example:

```bash
/home/ubuntu
```

---

## /var

Logs and variable files.

Example:

```bash
/var/log
```

---

## /tmp

Temporary files.

---

## /boot

Kernel and bootloader files.

---

## /dev

Device files.

Example:

```bash
/dev/sda
```

---

## /proc

Process and kernel information.

Example:

```bash
cat /proc/cpuinfo
```

---

# Useful Commands

## pwd

Show current directory.

```bash
pwd
```

---

## df -h

Show disk usage.

```bash
df -h
```

---

## du -sh

Show directory size.

```bash
du -sh folder/
```

---

# Summary

Everything in Linux is treated as a file.