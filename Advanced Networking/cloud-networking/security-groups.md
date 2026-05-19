# Security Groups

## Overview

Security Groups act as virtual firewalls for cloud resources.

---

## Why Important

Controls:
- inbound traffic
- outbound traffic

Used in:
- EC2
- Kubernetes nodes
- databases

---

## Rules

### Inbound

Who can access your server.

### Outbound

Where your server can go.

---

## Example Rule

Allow SSH:

```text
Port: 22
Source: 0.0.0.0/0
```

---

## Key Features

- stateful
- allow rules only
- attached to instances

---

## Real-world Example

Web server:

```text
Allow 80, 443
Block everything else
```

---

## DevOps Usage

Used to:
- secure cloud workloads
- restrict access
- control microservices

---

## Troubleshooting

### Scenario: Cannot connect to server

Check:
- SG rules
- port open
- IP allowed

---

## Interview Questions

1. What is Security Group?
2. Stateful vs stateless?
3. Difference between SG and NACL?

---

## Summary

Security Groups control instance-level traffic security.