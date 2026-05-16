# Archive & Compression

## tar - Create and Extract Archives

Tar packages files into a single archive. It doesn't compress by default but can be combined with compression tools.

### Create tar.gz (TAR + GZIP Compression)

**Basic example:**
```bash
tar -cvzf backup.tar.gz folder/
```

**Output:**
```
folder/
folder/file1.txt
folder/file2.txt
folder/subdir/
folder/subdir/file3.txt
```

**Detailed flags:**
```bash
tar -cvzf archive.tar.gz files_to_backup/
```
- `-c`: Create archive
- `-v`: Verbose (show files being processed)
- `-z`: Gzip compression
- `-f`: Filename of archive
- `files_to_backup/`: Directory to compress

### Extract tar.gz

**Basic extraction:**
```bash
tar -xvzf backup.tar.gz
```

**Extract to specific directory:**
```bash
tar -xvzf backup.tar.gz -C /tmp/
```

**Extract specific file:**
```bash
tar -xvzf backup.tar.gz folder/file1.txt
```

### Common Use Cases

**Create tar without compression (just archiving):**
```bash
tar -cvf backup.tar folder/
```

**Create tar with bzip2 compression (better compression):**
```bash
tar -cvjf backup.tar.bz2 folder/
```

**List contents without extracting:**
```bash
tar -tvzf backup.tar.gz
```

**Show archive size before extraction:**
```bash
tar -tvzf backup.tar.gz | tail -1
```

**Create incremental backup (only new/changed files):**
```bash
tar -cvzf backup_$(date +%Y%m%d).tar.gz --newer /tmp/last_backup folder/
```

**Exclude certain file types:**
```bash
tar -cvzf backup.tar.gz --exclude='*.log' --exclude='tmp/' folder/
```

**Exclude patterns:**
```bash
tar -cvzf backup.tar.gz --exclude-from=exclude.txt folder/
```

**Verify archive integrity:**
```bash
tar -tzf backup.tar.gz > /dev/null && echo "Archive OK" || echo "Archive corrupted"
```

### Before/After Example

**Before - Multiple commands to backup:**
```bash
gzip -r folder/
mv folder.gz backup.gz
```

**After - Single command with tar + gzip:**
```bash
tar -cvzf backup.tar.gz folder/
```

### Best Practices
- Use `-v` to verify files are being archived
- Use `-z` for gzip (fast), `-j` for bzip2 (better compression)
- Always use `-f` filename as the last flag
- Use `--exclude` to avoid archiving unnecessary files

---

## gzip

Compress individual files using GZIP compression.

### Compress Single File

**Basic compression:**
```bash
gzip file.txt
```

**Result:**
```
Before: file.txt (1.2 MB)
After:  file.txt.gz (250 KB)
```

### Common Use Cases

**Compress with verbose output:**
```bash
gzip -v file.txt
```
**Output:**
```
file.txt:	 79.2%
```

**Keep original file (don't delete):**
```bash
gzip -k file.txt  # Keeps both file.txt and file.txt.gz
```

**Compress multiple files:**
```bash
gzip file1.txt file2.txt file3.txt
```

**Set compression level (1-9, default 6):**
```bash
gzip -9 file.txt  # Maximum compression (slower)
gzip -1 file.txt  # Fast compression (less effective)
```

**Compress to standard output:**
```bash
gzip -c file.txt > file.txt.gz
```

**Show compression ratio:**
```bash
gzip -l file.txt.gz
```

### Before/After Example

**Before - file sizes:**
```bash
ls -lh
-rw-r--r-- 1 user user 15M app.log
```

**After - compression:**
```bash
gzip app.log
ls -lh
-rw-r--r-- 1 user user 2.3M app.log.gz
```

### Best Practices
- Use `-k` to keep original file for backup
- Use gzip for text files, less effective for already-compressed files
- Use `-9` for archival, `-1` for quick compression

---

## gunzip (gzip -d)

Extract GZIP compressed files.

### Decompress File

**Basic decompression:**
```bash
gunzip file.txt.gz
```

**Alternative syntax:**
```bash
gzip -d file.txt.gz
```

### Common Use Cases

**Decompress to different name:**
```bash
gunzip -c file.txt.gz > file_extracted.txt
```

**Decompress multiple files:**
```bash
gunzip file1.gz file2.gz file3.gz
```

**Keep compressed file:**
```bash
gunzip -k file.txt.gz  # Keeps both .gz and extracted
```

**Decompress with verbose output:**
```bash
gunzip -v file.txt.gz
```

### Before/After Example

**Before - compressed:**
```bash
ls -lh
-rw-r--r-- 1 user user 2.3M app.log.gz
```

**After - decompressed:**
```bash
gunzip app.log.gz
ls -lh
-rw-r--r-- 1 user user 15M app.log
```

---

## zip

Create ZIP archives (compatible with Windows).

### Create ZIP Archive

**Basic zip:**
```bash
zip files.zip file1.txt file2.txt
```

**Zip entire directory:**
```bash
zip -r backup.zip folder/
```

**Output:**
```
  adding: folder/ (stored 0%)
  adding: folder/file1.txt (deflated 75%)
  adding: folder/file2.txt (deflated 82%)
```

### Common Use Cases

**Exclude files while zipping:**
```bash
zip -r backup.zip folder/ -x "*.log" "tmp/*"
```

**Update existing zip (add new files):**
```bash
zip -u backup.zip newfile.txt
```

**Create with compression level:**
```bash
zip -9 -r backup.zip folder/  # Maximum compression
```

**Create split zip files (for storage limits):**
```bash
zip -r -s 100m backup.zip folder/  # 100MB chunks
```

**List zip contents:**
```bash
unzip -l backup.zip
```

**Test zip integrity:**
```bash
unzip -t backup.zip
```

**Create password-protected zip:**
```bash
zip -P mypassword secure.zip file.txt
```

**Encrypt zip file:**
```bash
zip -e -r secure.zip folder/  # Interactive password prompt
```

### Before/After Example

**Before - multiple tarballs:**
```bash
tar -czf backup1.tar.gz folder/
tar -czf backup2.tar.gz folder/
```

**After - single zip:**
```bash
zip -r backup.zip folder/
```

---

## unzip

Extract ZIP archives.

### Extract ZIP File

**Basic extraction:**
```bash
unzip files.zip
```

**Extract to specific directory:**
```bash
unzip files.zip -d /tmp/extracted/
```

### Common Use Cases

**List zip contents (preview):**
```bash
unzip -l files.zip
```

**Extract specific file:**
```bash
unzip files.zip "path/to/specific/file.txt"
```

**Overwrite existing files:**
```bash
unzip -o files.zip
```

**Skip existing files:**
```bash
unzip -n files.zip
```

**Extract with verbose output:**
```bash
unzip -v files.zip
```

**Test zip integrity:**
```bash
unzip -t files.zip
```

**Extract password-protected zip:**
```bash
unzip -P mypassword secure.zip
```

**Extract and delete zip after:**
```bash
unzip files.zip && rm files.zip
```

### Before/After Example

**Before - no visibility:**
```bash
unzip archive.zip  # Extracts to current directory
```

**After - controlled extraction:**
```bash
unzip -l archive.zip | head  # Preview first
unzip archive.zip -d ./extracted/  # Extract to specific folder
```

### Best Practices
- Use `-l` to preview contents before extracting
- Use `-t` to verify integrity before extracting large files
- Use `-d` to extract to specific directory

---

## Compression Comparison

| Command | Format | Compression | Speed | Use Case |
|---------|--------|-------------|-------|----------|
| tar -z | .tar.gz | Good | Fast | Default Linux backup |
| tar -j | .tar.bz2 | Better | Slower | Long-term archival |
| gzip | .gz | Good | Fast | Compressing single files |
| zip | .zip | Good | Fast | Cross-platform (Windows) |
| tar | .tar | None | Very Fast | Archiving without compression |

---

## Best Practices Summary

1. **For backups:** Use `tar -cvzf` for best compatibility
2. **For distribution:** Use `zip` for Windows compatibility
3. **For archival:** Use `tar -j` for better compression
4. **For logs:** Use `gzip` for individual file compression
5. **Always verify:** Use `-t` or `-v` to test before trusting archives