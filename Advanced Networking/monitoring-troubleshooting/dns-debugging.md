# DNS Debugging

## Overview

DNS issues are one of the most common production failures.

---

## Symptoms

- website not opening
- slow resolution
- NXDOMAIN errors

---

## Debug Tools

```bash
dig
nslookup
```

---

## Common Problems

### 1. NXDOMAIN

Domain does not exist.

---

### 2. SERVFAIL

DNS server failure.

---

### 3. Slow DNS

Resolver latency issue.

---

## Kubernetes DNS Debug

```bash
kubectl logs coredns
```

---

## Real-world Example

Service not reachable due to:
- wrong DNS entry
- missing record

---

## Interview Questions

1. What is NXDOMAIN?
2. How does DNS caching work?
3. Why DNS fails in Kubernetes?

---

## Summary

DNS debugging is critical in cloud-native systems.