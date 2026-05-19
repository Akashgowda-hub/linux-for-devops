# Load Balancers

## Overview

Load balancers distribute traffic across multiple servers.

---

## Why Important

Used for:
- scalability
- high availability
- fault tolerance

---

## Types

### 1. Application Load Balancer (ALB)
- Layer 7
- HTTP/HTTPS

### 2. Network Load Balancer (NLB)
- Layer 4
- TCP/UDP

---

## Architecture

User → Load Balancer → Multiple Servers

---

## Real-world Example

Website with 3 backend servers:

LB distributes traffic evenly.

---

## Health Checks

Ensures only healthy servers receive traffic.

---

## DevOps Usage

Used in:
- microservices
- Kubernetes ingress
- APIs

---

## Troubleshooting

### Scenario: Server not receiving traffic

Check:
- health check
- target group
- security group

---

## Interview Questions

1. What is load balancing?
2. ALB vs NLB?
3. What are health checks?

---

## Summary

Load balancers ensure scalability and reliability.