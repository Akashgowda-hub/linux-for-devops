# Public vs Private Subnets

## Overview

Subnets divide a VPC into smaller networks.

They are classified as:

- Public Subnet
- Private Subnet

---

## Public Subnet

A subnet that can access the Internet.

### Characteristics:
- has route to Internet Gateway
- used for load balancers
- exposed services

---

## Private Subnet

No direct internet access.

### Characteristics:
- uses NAT Gateway for outbound access
- used for databases
- used for backend services

---

## Architecture Example

```text
Internet
   ↓
Public Subnet
   ↓
Private Subnet
```

---

## Real-world Example

Frontend → Public Subnet  
Backend → Private Subnet  
Database → Private Subnet

---

## Route Tables

### Public Subnet

```text
0.0.0.0/0 → Internet Gateway
```

---

### Private Subnet

```text
0.0.0.0/0 → NAT Gateway
```

---

## DevOps Relevance

Used in:
- secure deployments
- microservices architecture
- Kubernetes node separation

---

## Troubleshooting

### Scenario: Private subnet has no internet

Check:
- NAT Gateway
- route table

---

## Interview Questions

1. Difference between public and private subnet?
2. Why use private subnet?
3. How does private subnet access internet?

---

## Summary

Subnet design controls security and traffic flow in cloud systems.