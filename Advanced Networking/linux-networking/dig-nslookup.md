# dig and nslookup

## Overview

Tools used for DNS troubleshooting.

---

## dig

Most powerful DNS tool.

```bash
dig google.com
```

---

## nslookup

Simpler DNS tool.

```bash
nslookup google.com
```

---

## Check Specific Record

```bash
dig A google.com
```

---

## Trace DNS

```bash
dig +trace google.com
```

---

## Real-world Example

Used when:
- website not opening
- DNS misconfigured
- Kubernetes service not resolving

---

## DNS Server Check

```bash
cat /etc/resolv.conf
```

---

## Troubleshooting

### Scenario 1: NXDOMAIN

Domain does not exist.

---

### Scenario 2: Slow DNS

Check resolver performance.

---

## Interview Questions

1. Difference between dig and nslookup?
2. What is NXDOMAIN?
3. How DNS resolution works?

---

## Summary

dig is essential for DNS debugging in production systems.