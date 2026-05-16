# File Commands

## ls - List Files and Directories

List files and directories with various options.

```bash
# Basic listing
ls

# Long format with all files including hidden
ls -la

# Sort by modification time (newest first)
ls -lt

# Sort by file size (largest first)
ls -lSr

# Human-readable file sizes
ls -lh

# Recursive listing
ls -R

# Show only directories
ls -d */
```

**Examples:**
```bash
# List all files with permissions
$ ls -la
total 24
drwxr-xr-x  3 user user 4096 May 16 10:30 .
drwxr-xr-x 10 user user 4096 May 15 09:45 ..
-rw-r--r--  1 user user 1024 May 16 10:30 file.txt
drwxr-xr-x  2 user user 4096 May 16 10:15 projects

# Find largest files in current directory
$ ls -lhS | head -10
```

---

## pwd - Print Working Directory

Show current directory path.

```bash
# Show current directory
pwd

# Show physical directory (resolve symlinks)
pwd -P
```

**Examples:**
```bash
$ pwd
/home/user/projects

$ pwd -P
/var/data/projects  # If /home/user/projects is a symlink
```

---

## cd - Change Directory

Navigate between directories.

```bash
# Change to specific directory
cd /home/user

# Change to home directory
cd ~ 
cd

# Change to previous directory
cd -

# Change to parent directory
cd ..

# Change to root
cd /

# Use environment variable
cd $HOME
```

**Examples:**
```bash
$ pwd
/home/user/projects/app

# Go to home
$ cd ~
$ pwd
/home/user

# Go back to previous directory
$ cd -
/home/user/projects/app

# Go to parent directory
$ cd ..
$ pwd
/home/user/projects
```

---

## touch - Create Empty File

Create new empty file or update modification time.

```bash
# Create single file
touch file.txt

# Create multiple files
touch file1.txt file2.txt file3.txt

# Update timestamp of existing file
touch file.txt

# Create file with specific timestamp
touch -t 202305161030 file.txt

# Create file without modifying if exists
touch -c existing.txt
```

**Examples:**
```bash
$ touch myfile.txt
$ ls -la myfile.txt
-rw-r--r-- 1 user user 0 May 16 10:30 myfile.txt

$ touch file{1..5}.txt
$ ls
file1.txt file2.txt file3.txt file4.txt file5.txt
```

---

## cat - Display File Contents

Display, concatenate, and create files.

```bash
# Display file contents
cat file.txt

# Display multiple files
cat file1.txt file2.txt

# Display with line numbers
cat -n file.txt

# Show end-of-line characters
cat -A file.txt

# Create file with content
cat > file.txt << EOF
Line 1
Line 2
Line 3
EOF
```

**Examples:**
```bash
$ cat file.txt
Hello DevOps
Welcome to Linux

$ cat -n config.txt
     1	server=production
     2	port=8080
     3	debug=false

# Combine multiple files
$ cat part1.txt part2.txt part3.txt > combined.txt
```

---

## cp - Copy Files and Directories

Copy files and directories.

```bash
# Copy file
cp source.txt destination.txt

# Copy directory recursively
cp -r source_dir/ dest_dir/

# Copy and preserve attributes
cp -p file.txt backup.txt

# Copy with verbose output
cp -v file.txt /backup/

# Copy multiple files
cp file1.txt file2.txt file3.txt /backup/

# Interactive copy (prompt if overwrite)
cp -i old.txt new.txt
```

**Examples:**
```bash
$ cp config.txt config.txt.bak
$ ls -la
-rw-r--r-- 1 user user 512 May 16 10:30 config.txt
-rw-r--r-- 1 user user 512 May 16 10:30 config.txt.bak

# Copy entire directory
$ cp -r /home/user/project /backup/project_backup

# Preserve timestamps
$ cp -p /var/log/app.log /backup/
$ ls -la /backup/app.log
-rw-r--r-- 1 user user 2048 May 15 09:15 app.log
```

---

## mv - Move or Rename Files

Move files to another location or rename them.

```bash
# Rename file
mv old_name.txt new_name.txt

# Move file to directory
mv file.txt /home/user/documents/

# Move multiple files
mv file1.txt file2.txt file3.txt /backup/

# Move with verbose output
mv -v file.txt /backup/

# Interactive move (prompt if overwrite)
mv -i old_location/file.txt new_location/
```

**Examples:**
```bash
$ mv config.txt config.bak
$ ls
config.bak

$ mv documents/old_file.txt ./
$ pwd
/home/user

$ mv *.log /var/log/archive/
$ ls /var/log/archive/
app.log system.log error.log
```

---

## rm - Delete Files

Remove files permanently.

```bash
# Delete file
rm file.txt

# Delete multiple files
rm file1.txt file2.txt

# Force delete without confirmation
rm -f file.txt

# Delete with confirmation prompt
rm -i file.txt

# Verbose output showing deleted files
rm -v file.txt
```

**Examples:**
```bash
$ rm temp.txt
$ echo "File deleted"

$ rm -i backup.txt
rm: remove write-protected file 'backup.txt'? y
$ echo "Removed"

# Delete all temp files
$ rm -f *.tmp
```

---

## rm -r - Delete Directories Recursively

Remove directories and all contents.

```bash
# Delete directory and contents
rm -r folder/

# Force delete without confirmation
rm -rf folder/

# Interactive deletion with confirmation
rm -ri folder/
```

**Examples:**
```bash
$ rm -r project_old/
$ echo "Directory deleted"

$ rm -rf /tmp/cache/*
$ echo "Cache cleared"

# Safer approach with confirmation
$ rm -ri old_backup/
rm: descend into directory 'old_backup'? y
rm: remove file 'old_backup/file1.txt'? y
```

---

## mkdir - Create Directories

Create new directories.

```bash
# Create single directory
mkdir project

# Create directory with specific permissions
mkdir -m 755 project

# Create parent directories if needed
mkdir -p /home/user/projects/app/src

# Create multiple directories
mkdir dir1 dir2 dir3
```

**Examples:**
```bash
$ mkdir my_project
$ ls -la
drwxr-xr-x 2 user user 4096 May 16 10:30 my_project

# Create nested structure
$ mkdir -p /opt/apps/myapp/config
$ ls -R /opt/apps/
/opt/apps/:
myapp

/opt/apps/myapp:
config
```

---

## rmdir - Remove Empty Directories

Remove empty directories only.

```bash
# Remove empty directory
rmdir folder

# Remove multiple empty directories
rmdir dir1 dir2 dir3

# Verbose output
rmdir -v folder

# Remove parent directories if empty
rmdir -p parent/child/subdir/
```

**Examples:**
```bash
$ rmdir empty_folder
$ echo "Removed empty directory"

# Error if directory not empty
$ rmdir non_empty_folder
rmdir: failed to remove 'non_empty_folder': Directory not empty

# Remove nested empty structure
$ rmdir -p a/b/c/
$ ls
# directories a, b, c are removed if all empty
```

---

## find - Search Files and Directories

Powerful tool to search files with various criteria.

```bash
# Find by name
find /home -name file.txt

# Find by name pattern
find /home -name "*.txt"

# Find files only (not directories)
find /home -type f -name "*.log"

# Find directories only
find /home -type d -name "backup"

# Find by size
find / -type f -size +100M  # Files larger than 100MB
find / -type f -size -10M   # Files smaller than 10MB

# Find by modification time
find / -type f -mtime -7    # Modified in last 7 days
find / -type f -mtime +30   # Not modified for 30 days

# Find by permissions
find / -type f -perm 644

# Find and execute command
find /tmp -type f -name "*.tmp" -delete
find / -type f -name "*.log" -exec gzip {} \;

# Combine multiple conditions
find / -type f -name "*.bak" -o -name "*.tmp"
```

**Examples:**
```bash
# Find all shell scripts
$ find . -name "*.sh" -type f
./deploy.sh
./backup.sh
./cleanup.sh

# Find files modified in last day
$ find /var/log -type f -mtime -1
/var/log/syslog
/var/log/auth.log

# Find and remove old log files
$ find /var/log -type f -name "*.log" -mtime +30 -delete
$ echo "Cleaned old logs"

# Find all files larger than 1GB
$ find / -type f -size +1G
/var/backup/large_backup.tar.gz
```

---

## Summary

File commands are fundamental for Linux navigation and file management. Combine them with pipes and redirection for powerful operations.