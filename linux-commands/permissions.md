# Permissions

File permissions control access to files and directories in Linux.

## Understanding Permission Format

```
-rwxr-xr-x  1 user group
│││││││││   │
││└─┬─┘││   └─ Hard link count
││  │  ││
││  │  └┴─ Group permissions (r-x)
││  │
││  └────── Owner permissions (rwx)
│└────────── File type (-=regular, d=directory, l=link)
└─────────── Others permissions (r-x)
```

**Permissions breakdown:**
- `r` (read) = 4
- `w` (write) = 2  
- `x` (execute) = 1

---

## chmod - Change File Permissions

Change file and directory permissions.

```bash
# Numeric notation (octal)
chmod 755 file.txt      # rwxr-xr-x
chmod 644 file.txt      # rw-r--r--
chmod 600 file.txt      # rw-------
chmod 777 file.txt      # rwxrwxrwx
chmod 000 file.txt      # ---------

# Symbolic notation
chmod u+x file.txt      # Add execute for user
chmod g+w file.txt      # Add write for group
chmod o-r file.txt      # Remove read for others
chmod a+r file.txt      # Add read for all
chmod u=rwx,g=rx file.txt  # Set specific permissions

# Recursive (directories)
chmod -R 755 directory/

# Verbose output
chmod -v 644 file.txt

# Remove all permissions
chmod a-rwx file.txt
```

**Common permission values:**
- `755` (rwxr-xr-x) — Executables, directories
- `644` (rw-r--r--) — Regular files, readable
- `600` (rw-------) — Private files
- `777` (rwxrwxrwx) — Full permissions (rarely used)
- `400` (r--------) — Read-only
- `700` (rwx------) — Owner only

**Examples:**
```bash
$ ls -la script.sh
-rw-r--r-- 1 user user 256 May 16 10:30 script.sh

$ chmod +x script.sh
$ ls -la script.sh
-rwxr-xr-x 1 user user 256 May 16 10:30 script.sh

# Multiple permissions
$ chmod u+rwx,g+rx,o-rwx file.txt
$ ls -la file.txt
-rwxr-x--- 1 user user 1024 May 16 10:30 file.txt

# Recursive on directory
$ chmod -R 755 /var/www/html
```

---

## chown - Change File Ownership

Change file owner and group.

```bash
# Change owner only
sudo chown user file.txt

# Change owner and group
sudo chown user:group file.txt

# Change group only (use colon)
sudo chown :group file.txt

# Recursive (directories)
sudo chown -R user:group directory/

# Verbose output
sudo chown -v user:group file.txt

# Change to match another file
sudo chown --reference=reference.txt file.txt
```

**Examples:**
```bash
$ ls -la myfile.txt
-rw-r--r-- 1 john users 512 May 16 10:30 myfile.txt

$ sudo chown root:root myfile.txt
$ ls -la myfile.txt
-rw-r--r-- 1 root root 512 May 16 10:30 myfile.txt

# Change owner of all files in directory
$ sudo chown -R www-data:www-data /var/www/html
$ ls -la /var/www/html
total 8
drwxr-xr-x 2 www-data www-data 4096 May 16 10:30 .
drwxr-xr-x 3 root     root     4096 May 16 10:15 ..
-rw-r--r-- 1 www-data www-data  342 May 16 10:30 index.html

# Change only group
$ sudo chown :docker myfile.txt
```

---

## chgrp - Change Group Ownership

Change only the group ownership of files.

```bash
# Change group
sudo chgrp groupname file.txt

# Change group of multiple files
sudo chgrp groupname file1.txt file2.txt

# Recursive (directories)
sudo chgrp -R groupname directory/

# Verbose output
sudo chgrp -v groupname file.txt
```

**Examples:**
```bash
$ ls -la project.txt
-rw-r--r-- 1 john john 256 May 16 10:30 project.txt

$ sudo chgrp developers project.txt
$ ls -la project.txt
-rw-r--r-- 1 john developers 256 May 16 10:30 project.txt

# Recursive group change
$ sudo chgrp -R docker /opt/docker-app
```

---

## File Types

Different file types in Linux:

| Symbol | Type | Meaning |
|--------|------|---------|
| `-` | Regular file | Normal files |
| `d` | Directory | Folder/Directory |
| `l` | Symlink | Symbolic link |
| `b` | Block device | Block device (disk) |
| `c` | Char device | Character device |
| `p` | Named pipe | Pipe |
| `s` | Socket | Socket |

**Examples:**
```bash
$ ls -la /
total 77
drwxr-xr-x  19 root root   4096 May 16 10:30 .
drwxr-xr-x  19 root root   4096 May 16 10:30 ..
-rw-r--r--   1 root root   1024 May 15 09:00 README.md
d-wxr-xr-x   2 root root   4096 May 16 10:15 bin
lrwxrwxrwx   1 root root      7 May 01 12:00 sbin -> bin
brw-rw----   1 root disk 259,  0 May 16 10:30 sda
crw-rw-rw-   1 root tty   4,   0 May 16 10:30 tty0
```

---

## Special Permissions

### setuid (Set User ID)

Executable runs with owner's permissions instead of executor's.

```bash
# Set setuid
chmod u+s file

# View setuid
ls -la
# Shows: -rwsr-xr-x  (s in owner execute position)

# Example: passwd command
$ ls -la /usr/bin/passwd
-rwsr-xr-x 1 root root 59976 May 01 12:00 /usr/bin/passwd
# Anyone can change password using root's privileges
```

### setgid (Set Group ID)

Files inherit parent directory's group; directories create files with group ownership.

```bash
# Set setgid
chmod g+s directory/

# View setgid
ls -la
# Shows: drwxr-sr-x  (s in group execute position)

# Example:
$ mkdir project
$ chmod g+s project
$ ls -la | grep project
drwxr-sr-x 2 user developers 4096 May 16 10:30 project
# Files created in project will have developers group
```

### Sticky Bit

Only owner can delete files in the directory.

```bash
# Set sticky bit
chmod +t directory/

# View sticky bit
ls -la
# Shows: drwxrwxrwt  (t in others execute position)

# Example: /tmp directory
$ ls -la / | grep tmp
drwxrwxrwt 11 root root 4096 May 16 10:45 tmp
# Anyone can create files, but only root can delete others' files
```

**Combined special permissions:**
```bash
# All three special permissions
chmod 7755 file
# 7 = setuid(4) + setgid(2) + sticky bit(1)
# 755 = rwxr-xr-x

# Symbolic
chmod u+s,g+s,+t directory
```

---

## Permission Examples

### Typical Web Server Permissions

```bash
# Web root directory
sudo chmod 755 /var/www/html
sudo chown www-data:www-data /var/www/html

# Web files
sudo chmod 644 /var/www/html/*.html
sudo chmod 644 /var/www/html/*.css
sudo chmod 644 /var/www/html/*.js

# Uploaded files (with setgid)
sudo chmod g+s /var/www/uploads
sudo chmod 775 /var/www/uploads
```

### Secure Private Files

```bash
# SSH keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/authorized_keys

# Config files
chmod 600 /etc/mysql/my.cnf
chmod 600 ~/.ssh/config

# Logs (readable by owner only)
chmod 600 /var/log/sensitive.log
```

### Shared Team Directory

```bash
# Create shared directory
mkdir /home/teamshare
sudo chown :developers /home/teamshare
sudo chmod 2770 /home/teamshare
# Files created by any team member will have developers group
```

---

## Viewing Permissions

```bash
# Long format (shows all details)
ls -la

# Numeric format
stat -c '%a %n' file.txt

# Recursive with permissions
find . -type f -exec ls -la {} \;

# Find files with specific permissions
find . -perm 644

# Find files without execute
find . -type f ! -perm /111
```

**Examples:**
```bash
$ ls -la /usr/bin/passwd /bin/bash
-rwsr-xr-x 1 root root 59976 May 01 12:00 /usr/bin/passwd
-rwxr-xr-x 1 root root  1183 May 01 12:00 /bin/bash

$ stat -c '%A %a %n' /etc/passwd
-rw-r--r-- 644 /etc/passwd

$ find . -perm 755
./script.sh
./deploy.sh
```

---

## Summary

Understanding and managing file permissions is crucial for Linux security. Always follow the principle of least privilege: grant only necessary permissions.