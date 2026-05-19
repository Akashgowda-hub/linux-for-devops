# NAT Gateway

## Overview

NAT Gateway allows private subnets to access the internet securely.

---

## Why NAT Exists

Private subnet cannot directly access internet.

NAT solves:
- outbound internet access
- keeps private IP hidden

---

## How It Works

Private subnet → NAT Gateway → Internet

---

## Key Difference

| Feature | NAT | IGW |
|---|---|---|
| Inbound | No | Yes |
| Outbound | Yes | Yes |
| Used in | Private subnet | Public subnet |

---

## Real-world Example

Backend server needs:
- software updates
- external API access

But should not be publicly exposed.

---

## Architecture

```text
Private EC2 → NAT → Internet Gateway → Internet
```

---

## DevOps Usage

Used in:
- Kubernetes worker nodes
- backend services
- databases

---

## Troubleshooting

### Scenario: No internet in private subnet

Check:
- NAT Gateway exists
- route table configured

---

## Interview Questions

1. What is NAT Gateway?
2. Why not use IGW for private subnet?
3. NAT vs Proxy?

---

## Summary

NAT Gateway enables secure outbound internet access for private systems.