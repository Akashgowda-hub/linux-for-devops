# Shell Scripting

## Shebang

```bash
#!/bin/bash
```

Always use shebang to specify the interpreter.

---

## Safety Flags

### Exit on Error

```bash
set -e  # Exit if any command fails
```

### Debug Mode

```bash
set -x  # Print each command before execution
```

### Pipeline Failure Detection

```bash
set -o pipefail  # Fail if any command in pipe fails
```

### Combined Safety

```bash
#!/bin/bash
set -euo pipefail  # Best practice: exit on error, undefined vars, pipe failures
```

---

## Variables & Parameter Expansion

### Basic Variables

```bash
name="devops"
echo $name
echo ${name}  # Safer: explicit variable boundary
```

### Variable Types

```bash
# String
message="Hello DevOps"

# Integer
count=5

# Array
servers=("web1" "web2" "web3")
echo ${servers[0]}  # web1
echo ${servers[@]}  # All elements
echo ${#servers[@]}  # Array length
```

### Parameter Expansion

```bash
# Default value
echo ${var:-"default_value"}

# Assign default if unset
var=${var:="default"}

# Error if unset
${var:?"Error: var is required"}

# Remove suffix/prefix
filename="backup.tar.gz"
echo ${filename%.*}   # backup.tar
echo ${filename##*.}  # gz
```

---

## Conditionals

### If Statement

```bash
if [ $? -eq 0 ]; then
  echo "Success"
elif [ $? -eq 1 ]; then
  echo "Error"
else
  echo "Unknown"
fi
```

### Test Operators

```bash
# String comparison
[ "$name" = "devops" ]   # String equals
[ "$name" != "test" ]    # String not equals
[ -z "$name" ]           # String is empty
[ -n "$name" ]           # String is not empty

# Numeric comparison
[ $count -eq 5 ]         # Equal
[ $count -gt 3 ]         # Greater than
[ $count -lt 10 ]        # Less than

# File tests
[ -f /etc/passwd ]       # File exists
[ -d /home ]             # Directory exists
[ -r /etc/passwd ]       # Readable
[ -w /home ]             # Writable
[ -x /usr/bin/ls ]       # Executable
```

### Case Statement

```bash
case "$1" in
  start)
    echo "Starting service..."
    ;;
  stop)
    echo "Stopping service..."
    ;;
  *)
    echo "Usage: $0 {start|stop|restart}"
    exit 1
    ;;
esac
```

---

## Loops

### For Loop

```bash
# Simple list
for i in 1 2 3; do
  echo $i
done

# Array iteration
servers=("web1" "web2" "web3")
for server in "${servers[@]}"; do
  echo "Processing $server"
done

# C-style loop
for ((i=1; i<=5; i++)); do
  echo $i
done

# Range
for i in {1..5}; do
  echo $i
done
```

### While Loop

```bash
count=1
while [ $count -le 5 ]; do
  echo "Iteration $count"
  ((count++))
done
```

### Until Loop

```bash
count=1
until [ $count -gt 5 ]; do
  echo $count
  ((count++))
done
```

---

## Functions

### Basic Function

```bash
greet() {
  echo "Hello, $1!"
}

greet "DevOps"
```

### Function with Return Value

```bash
check_file() {
  if [ -f "$1" ]; then
    return 0  # Success
  else
    return 1  # Failure
  fi
}

if check_file "/etc/passwd"; then
  echo "File exists"
fi
```

### Function with Local Variables

```bash
calculate() {
  local x=$1
  local y=$2
  local sum=$((x + y))
  echo $sum
}

result=$(calculate 5 3)
echo "Result: $result"
```

---

## Input & Output

### Reading Input

```bash
# Command line arguments
echo "Script name: $0"
echo "First arg: $1"
echo "All args: $@"
echo "Argument count: $#"

# Read from user
read -p "Enter your name: " name
echo "Hello, $name"

# Read multi-line input
read -r -d '' multiline << 'EOF'
This is
a multiline
string
EOF
```

### Output Redirection

```bash
# Redirect stdout to file
echo "log entry" > log.txt
echo "append log" >> log.txt

# Redirect stderr
command 2> error.txt

# Redirect both stdout and stderr
command &> output.txt

# Discard output
command > /dev/null 2>&1
```

---

## Practical Examples

### Example 1: System Health Check

```bash
#!/bin/bash
set -euo pipefail

echo "=== System Health Check ==="

# Check disk usage
disk_usage=$(df -h / | awk 'NR==2 {print $5}' | cut -d'%' -f1)
echo "Disk usage: $disk_usage%"

if [ "$disk_usage" -gt 80 ]; then
  echo "⚠️  WARNING: Disk usage is high!"
fi

# Check memory
free_mem=$(free -h | awk 'NR==2 {print $7}')
echo "Available memory: $free_mem"

# Check load average
load=$(uptime | awk -F'load average:' '{print $2}')
echo "Load average: $load"

echo "=== Check Complete ==="
```

### Example 2: Backup Script

```bash
#!/bin/bash
set -euo pipefail

BACKUP_DIR="/backup"
SOURCE_DIR="/home/user/data"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/backup_$DATE.tar.gz"

if [ ! -d "$SOURCE_DIR" ]; then
  echo "Error: Source directory not found!"
  exit 1
fi

echo "Starting backup of $SOURCE_DIR..."
tar -czf "$BACKUP_FILE" "$SOURCE_DIR"

if [ -f "$BACKUP_FILE" ]; then
  echo "✓ Backup successful: $BACKUP_FILE"
  ls -lh "$BACKUP_FILE"
else
  echo "✗ Backup failed!"
  exit 1
fi
```

### Example 3: Service Restart with Retry

```bash
#!/bin/bash
set -euo pipefail

SERVICE="nginx"
MAX_RETRIES=3
RETRY_DELAY=5

restart_service() {
  local service=$1
  local retry=0
  
  while [ $retry -lt $MAX_RETRIES ]; do
    echo "Attempting to restart $service (attempt $((retry+1))/$MAX_RETRIES)..."
    
    if systemctl restart "$service" 2>/dev/null; then
      echo "✓ $service restarted successfully"
      return 0
    fi
    
    ((retry++))
    if [ $retry -lt $MAX_RETRIES ]; then
      echo "⏳ Waiting ${RETRY_DELAY}s before retry..."
      sleep $RETRY_DELAY
    fi
  done
  
  echo "✗ Failed to restart $service after $MAX_RETRIES attempts"
  return 1
}

restart_service "$SERVICE"
```

### Example 4: Log Parser

```bash
#!/bin/bash
set -euo pipefail

LOG_FILE="/var/log/auth.log"
ERROR_COUNT=0

if [ ! -f "$LOG_FILE" ]; then
  echo "Error: Log file not found: $LOG_FILE"
  exit 1
fi

echo "Analyzing $LOG_FILE..."

# Count different error types
while IFS= read -r line; do
  if [[ $line =~ "Failed password" ]]; then
    ((ERROR_COUNT++))
  fi
done < "$LOG_FILE"

echo "Total failed password attempts: $ERROR_COUNT"

# Show top 5 failed IPs
echo ""
echo "Top 5 IPs with failed attempts:"
grep "Failed password" "$LOG_FILE" | awk '{print $(NF-3)}' | sort | uniq -c | sort -rn | head -5
```

---

## Best Practices

1. **Always use `set -euo pipefail`** at the start of production scripts
2. **Quote variables**: `"$var"` not `$var` to handle spaces correctly
3. **Use function names** instead of inline logic for reusability
4. **Add comments** for complex logic
5. **Handle errors** with proper exit codes
6. **Test thoroughly** with different inputs and edge cases
7. **Use absolute paths** for commands in production scripts
8. **Validate inputs** before using them

---

## Summary

Effective shell scripting requires proper error handling, defensive programming, and clear structure. Always prioritize readability and maintainability.