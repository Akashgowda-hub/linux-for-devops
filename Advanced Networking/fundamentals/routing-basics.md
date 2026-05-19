# Routing Basics

## Overview

Routing is the process of forwarding packets from one network to another.

Routers determine the best path for data to travel across networks.

Without routing:
- Internet communication would not work
- Cloud networking would fail
- Kubernetes clusters could not communicate across nodes

---

## Why Routing Matters

Routing is essential for:

- Internet communication
- Cloud infrastructure
- Kubernetes networking
- VPNs
- Hybrid cloud
- Data centers

---

## What is a Router?

A router is a device that:
- connects networks
- forwards packets
- maintains routing tables

Example:

Home Router:
- LAN → Internet

Cloud Router:
- VPC → Internet Gateway

---

## Routing Table

A routing table decides where packets should go.

Example:

```bash
ip route
```

Output:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel
```

---

## Important Terms

| Term | Meaning |
|---|---|
| Default Gateway | Exit route to Internet |
| Next Hop | Next router |
| Route | Path to network |
| Metric | Route priority |

---

## Types of Routing

### Static Routing

Routes manually configured.

Example:

```bash
ip route add 10.0.0.0/24 via 192.168.1.1
```

Advantages:
- simple
- predictable

Disadvantages:
- hard to scale

---

### Dynamic Routing

Routers exchange routes automatically.

Protocols:
- OSPF
- BGP
- RIP

Used in:
- ISPs
- cloud providers
- enterprise networks

---

## Packet Flow Example

Laptop → Router → ISP → Internet → Google

At each router:
- destination IP checked
- best route selected
- packet forwarded

---

## Real-world Cloud Example

AWS VPC:

```text
10.0.0.0/16
```

Public subnet route table:

```text
0.0.0.0/0 → Internet Gateway
```

Private subnet:

```text
0.0.0.0/0 → NAT Gateway
```

---

## Linux Commands

### Show Routes

```bash
ip route
```

### Add Route

```bash
sudo ip route add 10.0.0.0/24 via 192.168.1.1
```

### Delete Route

```bash
sudo ip route del 10.0.0.0/24
```

### Trace Packet Path

```bash
traceroute google.com
```

---

## Kubernetes Relevance

Kubernetes networking depends heavily on routing.

Examples:
- pod-to-pod communication
- node networking
- service routing

CNI plugins manage routes between pods.

---

## Troubleshooting Scenarios

### Scenario 1: No Internet Access

Check:

```bash
ip route
```

Possible causes:
- missing default gateway
- wrong subnet
- bad route

---

### Scenario 2: Cannot Reach Remote Network

Use:

```bash
traceroute
```

Possible causes:
- firewall
- missing route
- VPN issue

---

## Interview Questions

1. What is routing?
2. Difference between static and dynamic routing?
3. What is default gateway?
4. What is a routing table?
5. What happens if no route exists?

---

## Summary

Routing enables communication between networks and forms the backbone of Internet and cloud networking.