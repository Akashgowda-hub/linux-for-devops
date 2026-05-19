# Internet Gateway (IGW)

## Overview

An Internet Gateway allows communication between VPC and the Internet.

---

## Why It Matters

Without IGW:
- public subnet cannot access internet
- load balancers cannot serve traffic
- websites cannot be exposed

---

## How It Works

VPC → Internet Gateway → Internet

---

## Route Table Requirement

```text
0.0.0.0/0 → IGW
```

---

## Real-world Example

Public web server:

User → IGW → EC2 instance

---

## Key Points

- attached to VPC
- highly available
- horizontally scaled by AWS

---

## DevOps Usage

Used for:
- web servers
- APIs
- ingress traffic

---

## Troubleshooting

### Scenario: No internet access

Check:
- IGW attached?
- route table?
- security group?

---

## Interview Questions

1. What is Internet Gateway?
2. Can private subnet use IGW?
3. Difference between IGW and NAT?

---

## Summary

IGW enables inbound and outbound internet traffic for public subnets.