# Permissions

## chmod

Change file permissions.

```bash
chmod 754 file.txt
```

---

# Permission Meaning

| Number | Permission |
|--------|------------|
| 7 | rwx |
| 5 | r-x |
| 4 | r-- |

---

## chown

Change file ownership.

```bash
sudo chown user:group file.txt
```

---

## chgrp

Change group ownership.

```bash
sudo chgrp developers file.txt
```

---

# File Types

| Symbol | Meaning |
|--------|---------|
| - | Regular file |
| d | Directory |
| l | Symbolic link |

---

## setuid

Executable runs with owner permissions.

Example:

```bash
-rwsr-xr-x
```

---

## setgid

Files inherit parent directory group.

Example:

```bash
drwxr-sr-x
```