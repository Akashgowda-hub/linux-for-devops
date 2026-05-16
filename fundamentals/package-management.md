# Package Management

Package managers simplify software installation, updates, and removal by managing dependencies and versions automatically. Linux distributions use different package managers based on their origin.

---

## APT (Ubuntu/Debian)

APT (Advanced Packaging Tool) is the package manager for Debian-based systems like Ubuntu.

### Update Package Index

**Refresh package list (must do before installing):**
```bash
sudo apt update
```

**Output:**
```
Hit:1 http://archive.ubuntu.com/ubuntu jammy InRelease
Get:2 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
Get:3 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Fetched 2.4 MB in 3s
Reading package lists... Done
```

**Why update first:**
- Downloads latest package metadata
- Shows which packages have updates available
- Required before installing/upgrading

### Upgrade Packages

**Safe upgrade (respects dependencies):**
```bash
sudo apt upgrade
```

**Output:**
```
Reading package lists... Done
Building dependency tree... Done
The following packages will be upgraded:
  curl git nginx openssh-client openssh-server
5 upgraded, 0 newly installed
Need to get 45.6 MB of archives.
After this operation, 2,345 kB of additional disk space will be used.
Do you want to continue? [Y/n]
```

**Major version upgrade (removes packages if needed):**
```bash
sudo apt full-upgrade
```

**Check what would be upgraded without installing:**
```bash
apt list --upgradable
```

**Output:**
```
Listing... Done
curl/jammy-updates 7.81.0-1ubuntu1.10 amd64 [upgradable from: 7.81.0-1ubuntu1]
git/jammy-updates 1:2.34.1-1ubuntu1.5 amd64 [upgradable from: 1:2.34.1-1ubuntu1]
nginx/jammy-updates 1.18.0-6ubuntu14.2 amd64 [upgradable from: 1.18.0-6ubuntu14]
```

### Install Package

**Install single package:**
```bash
sudo apt install nginx
```

**Output:**
```
Reading package lists... Done
Building dependency tree... Done
The following NEW packages will be installed:
  libnginx-mod-http-geoip libnginx-mod-http-image-filter libnginx-mod-http-perl
  libnginx-mod-http-xslt-filter libnginx-mod-http-auth-request libnginx-mod-stream
  nginx nginx-common
0 upgraded, 8 newly installed, 0 to remove
Need to get 1,234 kB of archives.
After this operation, 5,678 kB of additional disk space will be used.
Do you want to continue? [Y/n] y
```

**Install multiple packages:**
```bash
sudo apt install curl wget git vim htop
```

**Install without confirmation:**
```bash
sudo apt install -y nginx  # No confirmation prompt
```

**Install specific version:**
```bash
apt list -a nginx  # Show all available versions
sudo apt install nginx=1.18.0-6ubuntu14
```

### Remove Package

**Remove package (keep config files):**
```bash
sudo apt remove nginx
```

**Purge package (remove everything):**
```bash
sudo apt purge nginx  # Removes package and config files
```

**Remove unused dependencies:**
```bash
sudo apt autoremove
```

**Output:**
```
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be REMOVED:
  libnginx-mod-http-geoip libnginx-mod-http-image-filter
  libnginx-mod-http-perl libnginx-mod-http-xslt-filter
0 upgraded, 0 newly installed, 2 to remove
After this operation, 2,345 kB disk space will be freed.
Do you want to continue? [Y/n]
```

### Search Package

**Search for package:**
```bash
apt search nginx
```

**Output:**
```
Sorting... Done
Full Text Search... Done
nginx/jammy 1.18.0-6ubuntu14.3 amd64
  A free, open-source, high-performance HTTP server and reverse proxy, as well...

nginx-core/jammy 1.18.0-6ubuntu14.3 amd64
  nginx web/proxy server (core version)

nginx-full/jammy 1.18.0-6ubuntu14.3 amd64
  nginx web/proxy server (full version)

nginx-light/jammy 1.18.0-6ubuntu14.3 amd64
  nginx web/proxy server (light version)
```

**Show package information:**
```bash
apt show nginx
```

**Output:**
```
Package: nginx
Version: 1.18.0-6ubuntu14.3
Priority: optional
Section: web
Maintainer: Ubuntu Developers
Installed-Size: 612 kB
Depends: libc6, libluajit-5.1-2, libpcre3
Homepage: http://nginx.org/
Download-Size: 177 kB
APT-Sources: http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages
Description: A free, open-source, high-performance HTTP server and reverse proxy
```

### List Installed Packages

**Show all installed packages:**
```bash
apt list --installed
```

**Count installed packages:**
```bash
apt list --installed | wc -l
```

**Show installed packages of specific type:**
```bash
apt list --installed | grep "^python"
```

**Check if specific package is installed:**
```bash
apt list --installed | grep nginx
```

### Clean Package Cache

**Remove package cache (frees space):**
```bash
sudo apt clean
```

**Remove only outdated packages:**
```bash
sudo apt autoclean
```

---

## DPKG - Low-Level Package Manager

DPKG (Debian Package) is the lower-level tool that APT uses behind the scenes.

### Install from .deb File

**Install local .deb package:**
```bash
sudo dpkg -i package.deb
```

### List Installed Packages

**Show all installed packages:**
```bash
dpkg -l
```

**Output:**
```
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                          Version                  Architecture Description
+++-=============================-=======================-============-================================
ii  adduser                        3.118ubuntu2             all          add and remove users and groups
ii  apparmor                       3.0.4-2ubuntu2.3         amd64        user-space parser and utilities for
ii  base-files                     12.3.7ubuntu1            amd64        Debian base system miscellaneous files
ii  ca-certificates               20230311                 all          Common CA certificates
ii  curl                           7.81.0-1ubuntu1.10       amd64        command line tool for transferring data
```

**Count installed packages:**
```bash
dpkg -l | grep "^ii" | wc -l
```

**Search for package:**
```bash
dpkg -l | grep nginx
```

### Get Package Info

**Show detailed info:**
```bash
dpkg -s nginx
```

**Output:**
```
Package: nginx
Status: install ok installed
Priority: optional
Section: web
Installed-Size: 612
Maintainer: Ubuntu Developers
Architecture: amd64
Version: 1.18.0-6ubuntu14.3
Depends: libc6, libluajit-5.1-2, libpcre3
Conffiles:
 /etc/nginx/nginx.conf
 /etc/nginx/sites-available/default
```

**List files provided by package:**
```bash
dpkg -L nginx | head -20
```

---

## YUM/DNF (RHEL/CentOS/Fedora)

YUM (Yellowdog Updater Modified) and DNF (Dandified YUM) are package managers for Red Hat-based systems.

### Install Package

**Using DNF (newer):**
```bash
sudo dnf install nginx
```

**Using YUM (older):**
```bash
sudo yum install nginx
```

**Install multiple packages:**
```bash
sudo dnf install httpd mariadb-server firewalld
```

### Remove Package

**Remove package:**
```bash
sudo dnf remove nginx
```

**Remove with dependencies:**
```bash
sudo dnf autoremove
```

### Search Package

**Search for package:**
```bash
dnf search nginx
```

**Show package info:**
```bash
dnf info nginx
```

### List Installed Packages

**Show all installed packages:**
```bash
rpm -qa | head -20
```

**Count installed packages:**
```bash
rpm -qa | wc -l
```

**Find package:**
```bash
rpm -qa | grep nginx
```

### Update Packages

**Update all packages:**
```bash
sudo dnf update
```

**Update specific package:**
```bash
sudo dnf update nginx
```

---

## Common Package Management Tasks

### Find Which Package Provides a File

**On Debian/Ubuntu:**
```bash
apt-file search bin/python3
```

**Output:**
```
python3-minimal: /usr/bin/python3
python3-full: /usr/bin/python3
```

**On RHEL/CentOS:**
```bash
yum whatprovides /usr/bin/python3
```

### Check Package Dependencies

**On Debian/Ubuntu:**
```bash
apt show nginx | grep Depends
```

**Output:**
```
Depends: libc6, libluajit-5.1-2, libpcre3
```

**On RHEL/CentOS:**
```bash
rpm -qR nginx
```

### Hold Package (Prevent Updates)

**Prevent package from being upgraded:**
```bash
sudo apt-mark hold nginx
```

**Allow updates again:**
```bash
sudo apt-mark unhold nginx
```

---

## Package Management Workflows

### Before & After: Installing a Web Server

**Before - Manual:**
```bash
# Download from website
# Compile from source
./configure
make
sudo make install
# Manage dependencies manually
```

**After - Using package manager:**
```bash
sudo apt update
sudo apt install nginx
# Done - all dependencies installed automatically
```

### Example: System Update

**Full system update:**
```bash
sudo apt update       # Refresh packages
sudo apt upgrade      # Upgrade packages
sudo apt autoremove   # Remove unused
sudo apt autoclean    # Clean cache
sudo reboot           # If kernel updated
```

---

## Troubleshooting Package Issues

### Broken Dependencies

**Fix broken packages:**
```bash
sudo apt install -f
```

**Try to repair:**
```bash
sudo apt --fix-broken install
```

### Lock Files

**If apt is locked:**
```bash
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo apt update
```

### Check Repository Sources

**View configured repositories:**
```bash
cat /etc/apt/sources.list
```

**Add PPA (Personal Package Archive):**
```bash
sudo add-apt-repository ppa:user/ppa-name
sudo apt update
```

---

## Best Practices

1. **Always update before installing:**
   ```bash
   sudo apt update
   sudo apt install package-name
   ```

2. **Clean up regularly:**
   ```bash
   sudo apt autoremove && sudo apt autoclean
   ```

3. **Hold critical packages:**
   ```bash
   sudo apt-mark hold kernel-image
   ```

4. **Keep system updated:**
   ```bash
   sudo apt upgrade  # Weekly or monthly
   ```

5. **Check before major upgrades:**
   ```bash
   apt list --upgradable  # See what will change
   ```

---

## Package Manager Comparison

| Task | APT (Debian) | DNF (RHEL) |
|------|---|---|
| Update index | `apt update` | `dnf check-update` |
| Install | `apt install` | `dnf install` |
| Remove | `apt remove` | `dnf remove` |
| Search | `apt search` | `dnf search` |
| List installed | `apt list --installed` | `rpm -qa` |
| Show info | `apt show` | `dnf info` |
| Clean cache | `apt clean` | `dnf clean all` |

---

## Summary

Package managers are essential for:
- Installing software safely
- Managing dependencies automatically
- Keeping system updated
- Removing software cleanly
- Maintaining system integrity