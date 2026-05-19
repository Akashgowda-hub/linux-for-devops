# Load Balancing in SRE Systems

## Overview

Load balancing distributes traffic across multiple servers to ensure:
- high availability
- scalability
- fault tolerance

---

## Why It Matters

Without load balancing:
- one server gets overloaded
- system crashes under traffic spikes

---

## Types of Load Balancing

### 1. Layer 4 (Network)
- TCP/UDP level
- fast routing
- no content awareness

### 2. Layer 7 (Application)
- HTTP/HTTPS
- URL-based routing
- header-based routing

---

## Architecture

User → Load Balancer → Multiple Backend Servers

---

## Algorithms

| Method | Description |
|---|---|
| Round Robin | sequential |
| Least Connections | least busy server |
| IP Hash | sticky sessions |

---

## Real-world Example

E-commerce site:
- traffic spikes during sales
- LB distributes traffic evenly

---

## SRE Concerns

- health checks
- auto scaling integration
- failover handling

---

## Troubleshooting

### Issue: uneven traffic

Check:
- algorithm config
- backend health

---

## Interview Questions

1. What is load balancing?
2. L4 vs L7 difference?
3. What is health check?

---

## Summary

Load balancing ensures system stability under high traffic.