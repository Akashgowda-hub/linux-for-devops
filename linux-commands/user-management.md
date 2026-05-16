# User Management

## adduser - Create New User

Create a new user account interactively.

```bash
# Interactive user creation
sudo adduser username

# Create user with specific shell
sudo adduser --shell /bin/bash username

# Create user with home directory
sudo adduser --home /home/username username

# Create system user
sudo adduser --system --no-create-home appuser

# Add user with specific UID
sudo adduser --uid 1005 username
```

**Examples:**
```bash
$ sudo adduser john
Adding user `john' ...
Adding new group `john' (1001) ...
Adding new user `john' (1001) with group `john' ...
Creating home directory `/home/john' ...
Copying files from `/etc/skel' ...
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

$ grep john /etc/passwd
john:x:1001:1001:John,,,:home/john:/bin/bash
```

---

## deluser - Delete User

Remove user account.

```bash
# Delete user (keep home directory)
sudo deluser username

# Delete user and home directory
sudo deluser --remove-home username

# Delete user and all files
sudo deluser --remove-all-files username

# Batch delete users
sudo deluser user1
sudo deluser user2
```

**Examples:**
```bash
$ sudo deluser john
Removing user `john' ...
Warning: group `john' has no more members.
Done.

$ sudo deluser --remove-home testuser
Removing user `testuser' ...
Warning: group `testuser' has no more members.
```

---

## groups - Show User Groups

Display groups a user belongs to.

```bash
# Show current user's groups
groups

# Show specific user's groups
groups username

# Show all group information
getent group

# Show group members
cat /etc/group | grep groupname
```

**Examples:**
```bash
$ groups
john sudo docker staff

$ groups nginx
nginx : nginx www-data

$ getent group
root:x:0:
daemon:x:1:
bin:x:2:
sudo:x:27:john,admin
```

---

## usermod - Modify User Account

Modify user properties and group membership.

```bash
# Add user to group
sudo usermod -aG groupname username

# Add user to multiple groups
sudo usermod -aG sudo,docker,wheel username

# Change user's home directory
sudo usermod -d /newhome username

# Change user's shell
sudo usermod -s /bin/zsh username

# Lock user account
sudo usermod -L username

# Unlock user account
sudo usermod -U username

# Change user's primary group
sudo usermod -g groupname username
```

**Examples:**
```bash
$ sudo usermod -aG docker john
$ groups john
john : john docker sudo

$ sudo usermod -s /bin/zsh john
$ grep john /etc/passwd
john:x:1001:1001:John,,,:home/john:/bin/zsh

$ sudo usermod -L attackuser
$ passwd attackuser
passwd: The password for attackuser is locked.
```

---

## passwd - Change Password

Change user password.

```bash
# Change own password
passwd

# Change another user's password (requires sudo)
sudo passwd username

# Set password from command line (not recommended)
echo "password123" | sudo passwd --stdin username

# Expire password (force change on next login)
sudo passwd -e username

# Lock password
sudo passwd -l username

# Unlock password
sudo passwd -u username
```

**Examples:**
```bash
$ passwd
Changing password for user john.
Current password:
New password:
Retype new password:
passwd: all authentication tokens updated successfully.

$ sudo passwd john
New password:
Retype new password:
passwd: all authentication tokens updated successfully.

# Force password change on next login
$ sudo passwd -e john
```

---

## su - Switch User

Switch to another user account.

```bash
# Switch to another user
su username

# Switch to root
su -  # or su root

# Switch with login shell (load environment)
su - username

# Run command as another user
su - username -c "command"

# Execute without password (requires sudo)
sudo su - username
```

**Examples:**
```bash
$ su john
Password:
$ whoami
john

$ su - admin
$ echo $SHELL
/bin/bash

$ su - john -c "whoami"
Password:
john

$ sudo su - www-data
$ whoami
www-data
```

---

## sudo - Execute as Root

Execute commands with superuser privileges.

```bash
# Run single command as root
sudo command

# Login as root with su
sudo -i

# Run command as specific user
sudo -u username command

# List sudo privileges
sudo -l

# Edit sudoers file safely
sudo visudo

# Run command without password (if configured)
sudo -n command
```

**Examples:**
```bash
$ sudo systemctl restart nginx
[sudo] password for john:

$ sudo -l
User john may run the following commands:
    (ALL) ALL

$ sudo -u postgres psql -d mydb

# Grant sudo without password (DANGEROUS!)
$ sudo visudo
# Add: john ALL=(ALL) NOPASSWD: ALL
```

---

## id - Show User/Group Identity

Display UID, GID, and group memberships.

```bash
# Show current user's ID
id

# Show specific user's ID
id username

# Show only UID
id -u

# Show only GID
id -g

# Show all group IDs
id -G

# Show names instead of numbers
id -Gn
```

**Examples:**
```bash
$ id
uid=1001(john) gid=1001(john) groups=1001(john),4(adm),27(sudo),999(docker)

$ id root
uid=0(root) gid=0(root) groups=0(root)

$ id -u john
1001

$ id -Gn john
john adm sudo docker
```

---

## whoami - Show Current User

Display current logged-in user.

```bash
# Show current user
whoami

# Show current user with full info
id

# Show all logged-in users
who

# Show detailed login information
w
```

**Examples:**
```bash
$ whoami
john

$ sudo whoami
root

$ who
john   pts/0        2026-05-16 10:30
admin  pts/1        2026-05-16 11:15

$ w
 11:45:20 up 5 days,  3 users,  load average: 0.85, 0.92, 0.88
USER     TTY   FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
john     pts/0 192.168.1.100    10:30    5:20   0.23s  0.01s bash
```

---

## /etc/passwd - User Account File

System file containing user account information.

```bash
# View all users
cat /etc/passwd

# View specific user
grep username /etc/passwd

# Format: username:password:UID:GID:GECOS:home:shell
```

**File format explanation:**
```
john:x:1001:1001:John Doe,,,:/home/john:/bin/bash
│    │  │    │    │              │           └─ Login shell
│    │  │    │    └────────────────────────────── User info (GECOS)
│    │  │    └────────────────────────────────── Primary Group ID
│    │  └───────────────────────────────────── User ID
│    └──────────────────────────────────────── Password (x = hashed in /etc/shadow)
└─────────────────────────────────────────── Username
```

**Examples:**
```bash
$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
john:x:1001:1001:John Doe,,,:/home/john:/bin/bash
nginx:x:33:33:nginx user,,,:/var/www:/usr/sbin/nologin

# Count users
$ wc -l /etc/passwd
42 /etc/passwd

# Find system users (UID < 1000)
$ awk -F: '$3 < 1000 {print $1}' /etc/passwd
root
daemon
bin
sys
```

---

## /etc/shadow - Password File

Contains encrypted passwords (readable only by root).

```bash
# View shadow file (requires sudo)
sudo cat /etc/shadow

# Format: username:password:lastchange:min:max:warn:inactive:expire:reserved
```

**Examples:**
```bash
$ sudo cat /etc/shadow
root:*:19000:0:99999:7:::
john:$6$xK3kZ2x:19100:0:99999:7:::
```

---

## /etc/group - Group File

Contains group definitions.

```bash
# View all groups
cat /etc/group

# View specific group
grep groupname /etc/group

# Format: groupname:password:GID:members
```

**Examples:**
```bash
$ cat /etc/group | head -10
root:x:0:
daemon:x:1:
bin:x:2:
adm:x:4:john,admin
sudo:x:27:john
docker:x:999:john

# Find user's groups
$ grep john /etc/group
adm:x:4:john,admin
sudo:x:27:john
docker:x:999:john
```

---

## Summary

User management is critical for system security and resource allocation. Use these commands to create, modify, and manage user accounts and permissions effectively.