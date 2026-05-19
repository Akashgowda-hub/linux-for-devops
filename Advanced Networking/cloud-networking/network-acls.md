# Network ACLs

## Overview

Network ACLs are subnet-level firewalls.

---

## Difference from Security Groups

| Feature | SG | NACL |
|---|---|---|
| Level | Instance | Subnet |
| Stateful | Yes | No |
| Rules | Allow only | Allow + Deny |

---

## How It Works

Subnet → NACL → Instance → Security Group

---

## Rules

Evaluated in order.

---

## Example

Block specific IP:

```text
Deny 1.2.3.4/32
```

---

## DevOps Usage

Used for:
- extra security layer
- blocking malicious traffic

---

## Troubleshooting

### Scenario: Subnet not reachable

Check:
- NACL rules
- SG rules

---

## Interview Questions

1. SG vs NACL difference?
2. Why NACL is stateless?
3. Which is evaluated first?

---

## Summary

NACL provides subnet-level traffic control.