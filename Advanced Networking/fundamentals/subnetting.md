# Subnetting

## Overview

Subnetting divides large networks into smaller networks.

---

## Why Subnetting?

Benefits:
- security
- isolation
- efficient IP usage
- traffic management

---

## CIDR Notation

Example:

```text
192.168.1.0/24
```

/24 means:
- 24 bits network
- 8 bits host

---

## Common CIDRs

| CIDR | Hosts |
|---|---|
| /24 | 254 |
| /25 | 126 |
| /26 | 62 |

---

## Real-world Example

AWS VPC:

```text
10.0.0.0/16
```

Subnets:

```text
10.0.1.0/24
10.0.2.0/24
```

---

## Public vs Private Subnets

### Public

Has Internet access.

### Private

Internal communication only.

---

## Linux Commands

### Check Network

```bash
ip addr
```

### Check Routes

```bash
ip route
```

---

## Kubernetes Relevance

Kubernetes uses:
- Pod CIDR
- Service CIDR

---

## Troubleshooting

### IP Conflict

Symptoms:
- intermittent connectivity

---

## Interview Questions

1. What is CIDR?
2. Difference between /24 and /16?
3. What is broadcast address?

---

## Summary

Subnetting is essential for scalable cloud and enterprise networking.