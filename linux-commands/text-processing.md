# Text Processing

## grep

Search text patterns.

```bash
grep "error" app.log
```

---

## awk

Text processing and column extraction.

```bash
awk '{print $1}'
```

---

## sed

Stream editor.

```bash
sed 's/devops/linux/g' file.txt
```

---

## cut

Extract columns.

```bash
cut -d ":" -f1 /etc/passwd
```

---

## sort

Sort lines.

```bash
sort names.txt
```

---

## uniq

Remove duplicate lines.

```bash
uniq names.txt
```

---

## xargs

Build commands using input.

```bash
cat files.txt | xargs rm
```