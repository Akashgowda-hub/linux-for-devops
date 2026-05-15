# Shell Scripting

## Shebang

```bash
#!/bin/bash
```

---

## Debug Mode

```bash
set -x
```

---

## Exit on Error

```bash
set -e
```

---

## Pipeline Failure Detection

```bash
set -o pipefail
```

---

## Variables

```bash
name="devops"
echo $name
```

---

## If Condition

```bash
if [ $? -eq 0 ]
then
  echo "Success"
fi
```

---

## For Loop

```bash
for i in 1 2 3
do
  echo $i
done
```