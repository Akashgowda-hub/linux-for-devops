# Shell Basics

The shell allows users to interact with Linux using commands.

---

# Common Shells

- bash
- sh
- zsh
- ksh

---

# Check Current Shell

```bash
echo $SHELL
```

---

# Environment Variables

## View Variables

```bash
printenv
```

---

## PATH Variable

```bash
echo $PATH
```

---

# Aliases

Create command shortcuts.

```bash
alias ll='ls -la'
```

---

# Redirection

## Redirect Output

```bash
echo hello > file.txt
```

---

## Append Output

```bash
echo world >> file.txt
```

---

# Pipes

Combine commands.

```bash
cat file.txt | grep error
```

---

# Wildcards

```bash
*.txt
```

---

# Simple Script

```bash
#!/bin/bash

echo "Hello Linux"
```

Run script:

```bash
chmod +x script.sh
./script.sh
```

---

# Summary

Shell scripting helps automate Linux tasks.