# DNS

## Overview

DNS converts domain names into IP addresses.

Example:

```text
google.com → 142.x.x.x
```

DNS acts like the Internet phonebook.

---

## Why DNS Matters

Without DNS:
- websites become unreachable
- applications fail
- Kubernetes services break

DNS is critical for:
- cloud
- DevOps
- Kubernetes
- SRE
- service discovery

---

## DNS Components

| Component | Purpose |
|---|---|
| Resolver | Sends query |
| Root Server | Top-level DNS |
| TLD Server | .com, .org |
| Authoritative Server | Final answer |

---

## DNS Record Types

| Record | Purpose |
|---|---|
| A | IPv4 |
| AAAA | IPv6 |
| CNAME | Alias |
| MX | Mail |
| TXT | Verification |
| NS | Name server |

---

## DNS Resolution Flow

1. Browser cache
2. OS cache
3. Resolver
4. Root server
5. TLD server
6. Authoritative server

---

## Linux Commands

### dig

```bash
dig google.com
```

### nslookup

```bash
nslookup google.com
```

### host

```bash
host google.com
```

### Check Resolver

```bash
cat /etc/resolv.conf
```

---

## Kubernetes Relevance

CoreDNS provides:
- internal service discovery
- pod communication

Example:

```text
service.default.svc.cluster.local
```

---

## Troubleshooting

### DNS Timeout

Possible causes:
- firewall
- resolver failure
- network issue

---

### Wrong IP Returned

Possible causes:
- stale cache
- DNS poisoning
- wrong configuration

---

## Security

### DNS Spoofing

Fake DNS responses redirect users.

---

### DNSSEC

Adds integrity validation.

---

## Interview Questions

1. What happens when typing google.com?
2. Why does DNS use UDP?
3. Difference between recursive and iterative query?
4. What is CoreDNS?

---

## Summary

DNS enables modern service discovery and Internet communication.