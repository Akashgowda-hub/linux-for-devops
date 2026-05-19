# MAC Address vs IP Address

## Overview

MAC and IP addresses identify systems in networks.

---

## MAC Address

- Physical hardware address
- Layer 2
- Assigned to NIC

Example:

```text
00:1A:2B:3C:4D:5E
```

---

## IP Address

Logical address used for routing.

Example:

```text
192.168.1.10
```

---

## IPv4 vs IPv6

| Type | Example |
|---|---|
| IPv4 | 192.168.1.1 |
| IPv6 | 2001:db8::1 |

---

## Real-world Communication

Inside local network:
- MAC used

Across Internet:
- IP used

---

## ARP Relationship

ARP maps:
- IP → MAC

---

## Linux Commands

### Show MAC Address

```bash
ip link
```

### Show IP Address

```bash
ip addr
```

---

## Kubernetes Relevance

Pods receive:
- virtual IPs
- virtual MACs

---

## Interview Questions

1. Difference between MAC and IP?
2. Which layer uses MAC?
3. Can IP change?

---

## Summary

MAC identifies devices locally while IP enables routing globally.