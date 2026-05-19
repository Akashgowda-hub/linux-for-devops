# DNS Basics

## Overview

DNS (Domain Name System) converts domain names into IP addresses.

Example:

```text
google.com → 142.x.x.x
```

Without DNS:
- users would need to remember IP addresses
- modern Internet would be difficult to use

---

## Why DNS Matters

DNS is critical for:
- websites
- APIs
- cloud services
- Kubernetes service discovery
- email delivery

DNS failures can break entire systems.

---

## Core Components

| Component | Purpose |
|---|---|
| Resolver | Queries DNS |
| Root Server | Top-level DNS |
| TLD Server | .com, .org |
| Authoritative Server | Final answer |

---

## DNS Resolution Flow

When opening:

```text
https://google.com
```

Flow:

1. Browser cache
2. OS cache
3. DNS resolver
4. Root server
5. TLD server
6. Authoritative server
7. IP returned

---

## Common DNS Records

| Record | Purpose |
|---|---|
| A | IPv4 |
| AAAA | IPv6 |
| CNAME | Alias |
| MX | Mail |
| TXT | Verification |

---

## DNS Ports

| Port | Protocol |
|---|---|
| 53 | UDP |
| 53 | TCP |

UDP:
- normal queries

TCP:
- large responses
- zone transfers

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

### DNS Configuration

```bash
cat /etc/resolv.conf
```

---

## Real-world Example

When accessing Netflix:

DNS may return:
- nearest CDN
- regional edge server

This improves:
- speed
- latency
- scalability

---

## Kubernetes Relevance

Kubernetes uses CoreDNS.

Example:

```text
my-service.default.svc.cluster.local
```

Pods communicate using internal DNS.

---

## Cloud Relevance

AWS:
- Route53

Azure:
- Azure DNS

GCP:
- Cloud DNS

---

## Troubleshooting Scenarios

### Scenario 1: Website Not Resolving

Check:

```bash
dig google.com
```

Possible causes:
- DNS server failure
- firewall issue
- wrong resolver

---

### Scenario 2: Slow DNS

Use:

```bash
time nslookup google.com
```

Possible causes:
- network latency
- overloaded resolver

---

### Scenario 3: Kubernetes DNS Failure

Check:

```bash
kubectl get pods -n kube-system
kubectl logs -n kube-system coredns
```

---

## Security Considerations

### DNS Spoofing

Fake DNS responses redirect traffic.

---

### DNS Amplification

Used in DDoS attacks.

---

### DNSSEC

Adds authentication and integrity.

---

## Interview Questions

1. What happens when you type google.com?
2. Difference between recursive and iterative query?
3. Why does DNS use UDP?
4. What is CoreDNS?
5. Difference between A and CNAME record?

---

## Summary

DNS is the backbone of modern Internet and cloud service discovery.