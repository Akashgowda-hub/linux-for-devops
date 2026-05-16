# Text Processing

## grep

Search for text patterns in files and output matching lines.

### Basic Search
```bash
grep "error" app.log
```

**Output (if app.log contains):**
```
[ERROR] Connection failed at 10:30:45
[ERROR] Timeout occurred
ERROR: Invalid configuration
```

### Common Use Cases

**Case-insensitive search:**
```bash
grep -i "error" app.log
```

**Count matching lines:**
```bash
grep -c "error" app.log
```

**Show line numbers:**
```bash
grep -n "error" app.log
```

**Search recursively in directory:**
```bash
grep -r "TODO" src/
```

**Show context (2 lines before and after):**
```bash
grep -C 2 "error" app.log
```

**Invert match (lines that DON'T contain):**
```bash
grep -v "error" app.log
```

**Search multiple patterns:**
```bash
grep -E "(error|warning|critical)" app.log
```

**Show only filename containing pattern:**
```bash
grep -l "error" *.log
```

**Count total matches across files:**
```bash
grep -r "error" . | wc -l
```

### Before/After Example

**Before - viewing entire log:**
```bash
cat large_app.log | head -50  # Lots of irrelevant data
```

**After - finding errors quickly:**
```bash
grep -n "ERROR\|CRITICAL" large_app.log
```

### Best Practices
- Use `-n` to show line numbers for debugging
- Use `-i` for case-insensitive searches
- Use `-v` to find negative matches (e.g., "not containing")

---

## awk

Powerful text processing tool for column extraction and data transformation.

### Extract First Column
```bash
echo "john 25 engineer
jane 30 manager
bob 28 devops" | awk '{print $1}'
```

**Output:**
```
john
jane
bob
```

### Common Use Cases

**Extract multiple columns:**
```bash
awk '{print $1, $3}' employees.txt
```
**Output:**
```
john engineer
jane manager
bob devops
```

**Extract columns with custom delimiter:**
```bash
awk -F: '{print $1,$3}' /etc/passwd
```
**Output (first field and UID):**
```
root 0
daemon 1
bin 2
```

**Print lines matching condition:**
```bash
awk '$3 > 25' employees.txt  # Lines where 3rd field > 25
```

**Calculate sum of column:**
```bash
awk '{sum += $2} END {print "Total:", sum}' salaries.txt
```

**Count lines:**
```bash
awk 'END {print NR}' file.txt
```

**Print with formatted output:**
```bash
awk '{printf "Name: %-10s Age: %d\n", $1, $2}' employees.txt
```

**Extract column range:**
```bash
ls -la | awk '{print $1, $9}'  # Permissions and filename
```

**Before/After Example**

**Before - extracting from /etc/passwd manually:**
```bash
cat /etc/passwd | cut -d: -f1,3,5  # Limited capability
```

**After - using awk with complex operations:**
```bash
awk -F: '$3 > 999 {print $1 ": " $5}' /etc/passwd  # Regular users only
```

### Best Practices
- Use `-F` to specify field delimiter
- Use `END` block for aggregations
- Use `printf` for formatted output

---

## sed

Stream editor for filtering and transforming text.

### Basic Substitution
```bash
sed 's/devops/linux/g' file.txt
```

**Before:**
```
devops is about devops practices
devops tools for devops engineers
```

**After:**
```
linux is about linux practices
linux tools for linux engineers
```

### Common Use Cases

**In-place file editing:**
```bash
sed -i 's/old/new/g' file.txt
```

**Backup original and edit:**
```bash
sed -i.bak 's/old/new/g' file.txt  # Creates file.txt.bak
```

**Delete lines matching pattern:**
```bash
sed '/^#/d' config.conf  # Remove comments
```

**Replace on specific line:**
```bash
sed '5s/old/new/' file.txt  # Only line 5
```

**Replace in line range:**
```bash
sed '10,20s/old/new/g' file.txt  # Lines 10-20
```

**Print specific lines:**
```bash
sed -n '10,20p' file.txt  # Show lines 10-20
```

**Append text:**
```bash
sed '/pattern/a\New line text' file.txt
```

**Insert text:**
```bash
sed '/pattern/i\Text before pattern' file.txt
```

**Multiple substitutions:**
```bash
sed -e 's/old1/new1/' -e 's/old2/new2/' file.txt
```

### Before/After Example

**Before - manual log cleaning:**
```bash
grep -v "#" config.conf | grep -v "^$"  # Multiple pipes
```

**After - using sed:**
```bash
sed -e '/^#/d' -e '/^$/d' config.conf  # Single command
```

### Best Practices
- Use `-i` carefully; always backup with `-i.bak` first
- Use `-n` with `p` to print specific lines
- Escape special characters with backslash

---

## cut

Extract specific columns/fields from text.

### Extract Field by Delimiter
```bash
cut -d ":" -f1 /etc/passwd
```

**Output (usernames):**
```
root
daemon
bin
sys
sync
```

### Common Use Cases

**Extract multiple fields:**
```bash
cut -d ":" -f1,3,5 /etc/passwd
```

**Extract columns by position:**
```bash
cut -c 1-5 file.txt  # Characters 1-5
```

**Extract range of columns:**
```bash
cut -c 1-10,20-30 file.txt
```

**Extract from specific column onwards:**
```bash
cut -f4- names.csv  # Field 4 to end (comma-delimited)
```

**Complement (exclude fields):**
```bash
cut -d "," --complement -f2 data.csv  # All fields EXCEPT field 2
```

**Extract fields with custom delimiter:**
```bash
cut -d "|" -f1,3 data.txt
```

### Before/After Example

**Before - complex awk for simple extraction:**
```bash
awk -F: '{print $1}' /etc/passwd
```

**After - simpler with cut:**
```bash
cut -d: -f1 /etc/passwd
```

### Best Practices
- Use `-d` to specify delimiter
- Use `-f` for field-based extraction
- Use `-c` for character-based extraction

---

## sort

Sort lines alphabetically, numerically, or by custom criteria.

### Basic Sort
```bash
sort names.txt
```

**Before:**
```
charlie
alice
bob
david
```

**After:**
```
alice
bob
charlie
david
```

### Common Use Cases

**Numeric sort (not alphabetic):**
```bash
sort -n numbers.txt
```
**Before:**
```
100
2
30
5
```
**After:**
```
2
5
30
100
```

**Reverse sort:**
```bash
sort -r names.txt  # Z to A
```

**Unique sort (remove duplicates):**
```bash
sort -u names.txt
```

**Sort by specific field:**
```bash
sort -t: -k3 -n /etc/passwd  # Sort by UID field
```

**Sort by multiple fields:**
```bash
sort -t, -k2,2 -k1,1 data.csv  # Primary: field 2, Secondary: field 1
```

**Sort descending numeric:**
```bash
sort -rn ages.txt
```

**Check if file is sorted:**
```bash
sort -c file.txt && echo "Sorted" || echo "Not sorted"
```

### Before/After Example

**Before - sorting IPs (alphabetically, wrong):**
```bash
sort ips.txt  # 192.168.10.1 comes after 192.168.2.1
```

**After - numeric sort:**
```bash
sort -t. -k1,1n -k2,2n -k3,3n -k4,4n ips.txt
```

### Best Practices
- Use `-n` for numeric sorting
- Use `-t` and `-k` for field-based sorting
- Use `-u` to remove duplicates while sorting

---

## uniq

Remove or report duplicate consecutive lines.

### Remove Duplicates
```bash
uniq names.txt
```

**Before:**
```
alice
alice
bob
bob
bob
charlie
alice
```

**After:**
```
alice
bob
charlie
alice
```

**Note:** Only removes consecutive duplicates!

### Common Use Cases

**Count occurrences:**
```bash
uniq -c names.txt
```

**Show only duplicates:**
```bash
uniq -d names.txt  # Lines that appear more than once
```

**Show only unique lines:**
```bash
uniq -u names.txt  # Lines appearing exactly once
```

**Case-insensitive comparison:**
```bash
uniq -i names.txt
```

**Skip first N fields:**
```bash
uniq -f 2 data.txt  # Ignore first 2 fields
```

**Combined with sort (remove all duplicates, not just consecutive):**
```bash
sort names.txt | uniq
```

### Before/After Example

**Before - seeing file with duplicates:**
```bash
alice
alice
bob
charlie
charlie
```

**After - getting frequency count:**
```bash
sort names.txt | uniq -c
```
**Output:**
```
      2 alice
      1 bob
      2 charlie
```

### Best Practices
- Use `sort | uniq` to remove ALL duplicates
- Use `uniq -c` to count frequencies
- Remember uniq only works on consecutive duplicates

---

## xargs

Build and execute commands from standard input.

### Basic Usage - Remove Files
```bash
echo "file1.txt file2.txt file3.txt" | xargs rm
```

### Common Use Cases

**Pass multiple arguments to command:**
```bash
cat filelist.txt | xargs touch  # Create multiple files
```

**Limit number of arguments per command:**
```bash
cat filelist.txt | xargs -n 2 ls -la  # Process 2 files at a time
```

**Execute with verbose output:**
```bash
echo "file1 file2 file3" | xargs -v rm
```

**Use different delimiter:**
```bash
echo "file1:file2:file3" | xargs -d: rm
```

**Replace placeholder in command:**
```bash
find . -name "*.log" | xargs -I {} mv {} ./logs/  # {} is placeholder
```

**Parallel execution (4 processes):**
```bash
cat filelist.txt | xargs -P 4 -I {} process_file {}
```

**Null delimiter (handles spaces in filenames):**
```bash
find . -type f -print0 | xargs -0 ls -la
```

**Show what will execute without running:**
```bash
echo "file1 file2" | xargs -p rm  # Asks for confirmation
```

### Before/After Example

**Before - processing many files (loop):**
```bash
for file in $(cat files.txt); do
  process_file "$file"
done
```

**After - using xargs (parallel, faster):**
```bash
cat files.txt | xargs -P 4 -I {} process_file {}
```

### Best Practices
- Use `-0` with `find -print0` for handling spaces in filenames
- Use `-I {}` for custom placeholder
- Use `-P` for parallel execution on multiple cores
- Use `-p` for confirmation before executing