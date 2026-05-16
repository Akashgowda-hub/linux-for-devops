# Shell Basics

The shell is a command-line interface that allows users to interact with Linux. It interprets commands and communicates with the kernel.

---

## Common Shells

Different shells offer different features and syntax:

- **bash** - Bourne Again Shell (most common, default on most Linux systems)
- **sh** - POSIX shell (minimal, portable)
- **zsh** - Z Shell (enhanced features, modern)
- **ksh** - Korn Shell (advanced scripting)
- **fish** - Friendly Interactive Shell (user-friendly)

---

## Check Current Shell

**Display your current shell:**
```bash
echo $SHELL
```

**Output:**
```
/bin/bash
```

**List all available shells on system:**
```bash
cat /etc/shells
```

**Output:**
```
/bin/sh
/bin/bash
/usr/bin/zsh
/bin/ksh
```

**Switch to different shell temporarily:**
```bash
zsh  # Starts zsh shell
bash  # Returns to bash
```

**Change default shell (permanent):**
```bash
chsh -s /bin/zsh
# Log out and log back in to see change
```

---

## Environment Variables

Environment variables are dynamic values that affect how processes behave.

### View All Variables

**Show all environment variables:**
```bash
printenv
```

**Output (abbreviated):**
```
HOME=/home/user
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
USER=user
SHELL=/bin/bash
LANG=en_US.UTF-8
TERM=xterm-256color
```

**Show specific variable:**
```bash
printenv HOME
```

**Output:**
```
/home/user
```

### PATH Variable

**Display PATH (list of directories where executables are searched):**
```bash
echo $PATH
```

**Output:**
```
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

**Add directory to PATH (current session):**
```bash
export PATH=$PATH:/usr/local/my-tools
```

**Add directory to PATH (permanent):**
```bash
# Add this line to ~/.bashrc
export PATH=$PATH:/usr/local/my-tools

# Then reload
source ~/.bashrc
```

**Verify command location:**
```bash
which python3
```

**Output:**
```
/usr/bin/python3
```

### Create Custom Variables

**Set variable (current session only):**
```bash
export MYAPP_ENV=production
echo $MYAPP_ENV
```

**Output:**
```
production
```

**Set permanent variable (in ~/.bashrc):**
```bash
# Add to file
export DATABASE_URL="postgres://localhost/mydb"
export API_PORT=3000

# Reload
source ~/.bashrc
```

**Use variable in commands:**
```bash
cd $HOME
echo "Environment: $MYAPP_ENV"
```

---

## Aliases

Create shortcuts for frequently used commands.

### Create Alias

**Create temporary alias (current session):**
```bash
alias ll='ls -la'
alias ll
```

**Output:**
```
alias ll='ls -la'
```

**Use the alias:**
```bash
ll
```

**Same as:**
```bash
ls -la
```

### Common Useful Aliases

```bash
# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias home='cd ~'

# Listing
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'

# Safety
alias rm='rm -i'  # Confirm before delete
alias cp='cp -i'  # Confirm before overwrite
alias mv='mv -i'  # Confirm before overwrite

# Shortcuts
alias grep='grep --color=auto'
alias python='python3'
alias gs='git status'
alias gc='git commit'
alias gp='git push'

# System
alias update='sudo apt update && sudo apt upgrade'
alias df='df -h'
alias du='du -sh'
```

### Make Aliases Permanent

**Add to shell configuration file:**
```bash
# For bash - add to ~/.bashrc
# For zsh - add to ~/.zshrc

alias ll='ls -la'
alias gc='git commit'
alias gs='git status'

# Reload
source ~/.bashrc  # or source ~/.zshrc
```

**View all aliases:**
```bash
alias
```

**Remove alias:**
```bash
unalias ll
```

---

## Redirection

Redirect command output to files or other commands.

### Redirect Output to File

**Redirect stdout (overwrite file):**
```bash
echo "Hello World" > file.txt
```

**View result:**
```bash
cat file.txt
```

**Output:**
```
Hello World
```

### Append Output to File

**Redirect stdout (append to file):**
```bash
echo "Line 2" >> file.txt
```

**View result:**
```bash
cat file.txt
```

**Output:**
```
Hello World
Line 2
```

### Redirect Standard Error

**Redirect stderr (error messages):**
```bash
ls /nonexistent/ 2> errors.log
```

**View error:**
```bash
cat errors.log
```

**Output:**
```
ls: cannot access '/nonexistent/': No such file or directory
```

**Redirect both stdout and stderr:**
```bash
command 2>&1 > output.log  # All output to file
```

### Redirect Input

**Read from file as input:**
```bash
sort < unsorted.txt
```

**Run commands from file:**
```bash
bash < script.sh
```

---

## Pipes

Connect commands together - output of one command becomes input of another.

### Basic Piping

**Example: Count lines in log file:**
```bash
cat app.log | wc -l
```

**Show only ERROR lines from log:**
```bash
cat app.log | grep ERROR
```

**Count errors:**
```bash
cat app.log | grep ERROR | wc -l
```

**Output:**
```
42
```

### Practical Pipe Examples

**Find and remove old files:**
```bash
find . -name "*.tmp" -type f | xargs rm
```

**List top 5 largest files:**
```bash
find . -type f -exec ls -lh {} \; | sort -k5 -h | tail -5
```

**Extract specific columns from CSV:**
```bash
cat data.csv | cut -d',' -f1,3 | sort | uniq
```

**Process user list:**
```bash
cat /etc/passwd | cut -d: -f1 | sort
```

**Find process and show details:**
```bash
ps aux | grep nginx
```

**Chain multiple commands:**
```bash
# Find log files, compress them, save list
find /var/log -name "*.log" | head -10 | xargs tar -czf archive.tar.gz | tee compressed.txt
```

---

## Wildcards

Use pattern matching to work with multiple files.

### Common Wildcards

**`*` - Match any characters:**
```bash
ls *.txt          # All .txt files
cp *.log backup/  # Copy all .log files
rm temp*          # Remove all files starting with "temp"
```

**`?` - Match single character:**
```bash
ls file?.txt      # Matches file1.txt, file2.txt, fileA.txt
ls ?.txt          # Matches a.txt, b.txt (single char)
```

**`[...]` - Match any character in brackets:**
```bash
ls [abc].txt      # Matches a.txt, b.txt, or c.txt
ls test[0-9].sh   # Matches test0.sh through test9.sh
ls [!t]*.txt      # Matches all .txt files NOT starting with 't'
```

**`{...}` - Brace expansion (create multiple strings):**
```bash
touch file{1,2,3}.txt           # Creates file1.txt, file2.txt, file3.txt
mkdir {docs,images,videos}      # Creates three directories
cp file.txt {backup1,backup2}/  # Copy to multiple directories
```

---

## Simple Shell Script

Create a basic shell script to automate tasks.

### Create Basic Script

**Create new script file:**
```bash
nano myscript.sh
```

**Add script content:**
```bash
#!/bin/bash

# This is a comment
echo "Hello Linux"
echo "Current user: $USER"
echo "Current directory: $PWD"
```

### Run the Script

**Make executable:**
```bash
chmod +x myscript.sh
```

**Run script:**
```bash
./myscript.sh
```

**Output:**
```
Hello Linux
Current user: ubuntu
Current directory: /home/ubuntu
```

### Practical Script Examples

**Example 1: Backup script**
```bash
#!/bin/bash

# Backup important files
BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d)

tar -czf $BACKUP_DIR/backup_$DATE.tar.gz /home/user/documents
echo "Backup completed: $BACKUP_DIR/backup_$DATE.tar.gz"
```

**Example 2: System health check**
```bash
#!/bin/bash

echo "=== System Health Check ==="
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h | grep -E "^/dev/"
echo "Memory Usage:"
free -h | grep Mem
echo "CPU Load:"
cat /proc/loadavg
```

**Example 3: Conditional script**
```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

if [ -f "$1" ]; then
    echo "File exists: $1"
    wc -l "$1"
else
    echo "File not found: $1"
fi
```

**Example 4: Loop script**
```bash
#!/bin/bash

# Create numbered backup files
for i in {1..5}; do
    touch backup_$i.txt
    echo "Created backup_$i.txt"
done
```

### Run Script with Arguments

**Script with parameters:**
```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"
```

**Run with arguments:**
```bash
./myscript.sh arg1 arg2 arg3
```

**Output:**
```
Script name: ./myscript.sh
First argument: arg1
Second argument: arg2
All arguments: arg1 arg2 arg3
Number of arguments: 3
```

---

## Shell Configuration Files

Understand and customize shell startup behavior.

### Bash Configuration Files

**`~/.bashrc`** - Loaded for non-login interactive shells
```bash
# Add aliases and functions
alias ll='ls -la'
export EDITOR=nano
```

**`~/.bash_profile`** - Loaded for login shells
```bash
# Usually sources .bashrc
if [ -f ~/.bashrc ]; then
    source ~/.bashrc
fi
```

**`/etc/bashrc`** - System-wide bash configuration
```bash
# Don't edit directly; use ~/.bashrc instead
```

### Reload Configuration

**Apply changes to current session:**
```bash
source ~/.bashrc
# or
. ~/.bashrc
```

**Log out and back in for full effect:**
```bash
exit  # Log out
# Log back in
```

---

## Summary

| Concept | Purpose | Example |
|---------|---------|---------|
| Environment Variables | Store system/user settings | `export PATH=$PATH:/usr/local/bin` |
| Aliases | Create command shortcuts | `alias ll='ls -la'` |
| Redirection | Send output to files | `echo "text" > file.txt` |
| Pipes | Connect commands | `cat file.txt \| grep error` |
| Wildcards | Pattern matching | `ls *.txt` |
| Scripts | Automate tasks | `./backup.sh` |

Shell scripting is a powerful way to automate Linux tasks and improve productivity.