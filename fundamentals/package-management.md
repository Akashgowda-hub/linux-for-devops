# Package Management

Package managers help install, update, and remove software.

---

# APT (Ubuntu/Debian)

## Update package index

```bash
sudo apt update
```

---

## Upgrade packages

```bash
sudo apt upgrade
```

---

## Install package

```bash
sudo apt install nginx
```

---

## Remove package

```bash
sudo apt remove nginx
```

---

## Search package

```bash
apt search nginx
```

---

## List installed packages

```bash
apt list --installed
```

---

# DPKG

Low-level package manager.

## Install package

```bash
sudo dpkg -i package.deb
```

---

## List installed packages

```bash
dpkg -l
```

---

# YUM/DNF (RHEL/CentOS)

## Install package

```bash
sudo yum install nginx
```

---

## Remove package

```bash
sudo yum remove nginx
```

---

## List installed packages

```bash
rpm -qa
```

---

# Summary

Package managers simplify software installation and updates.