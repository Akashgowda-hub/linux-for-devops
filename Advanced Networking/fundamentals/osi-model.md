# OSI Model

## Overview

The OSI (Open Systems Interconnection) model is a conceptual framework used to understand how network communication works between systems.

It divides communication into 7 layers.

---

## Why OSI Model Matters

The OSI model helps engineers:

- troubleshoot networking issues
- understand packet flow
- isolate failures
- design scalable systems

Used heavily in:
- DevOps
- SRE
- Cloud
- Kubernetes
- Security
- System Design

---

## OSI Layers

| Layer | Name | Examples |
|---|---|---|
| 7 | Application | HTTP, DNS |
| 6 | Presentation | SSL/TLS |
| 5 | Session | RPC |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP |
| 2 | Data Link | Ethernet, MAC |
| 1 | Physical | Cable, Fiber |

---

## Real-world Example

Opening:

https://google.com

Flow:

1. Browser creates HTTP request
2. TLS encryption applied
3. TCP connection established
4. IP routing performed
5. Ethernet frame created
6. Data transmitted over cable/WiFi

---

## Encapsulation

As data moves down layers:

| Layer | Data Unit |
|---|---|
| Application | Data |
| Transport | Segment |
| Network | Packet |
| Data Link | Frame |

---

## Packet Flow

Application
→ TCP
→ IP
→ Ethernet
→ Physical Network

---

## Layer-wise Troubleshooting

| Problem | Layer |
|---|---|
| Cable unplugged | Layer 1 |
| MAC issue | Layer 2 |
| Routing issue | Layer 3 |
| Port blocked | Layer 4 |
| DNS failure | Layer 7 |

---

## Linux Commands

### Check Interfaces

```bash
ip addr
```

### Check Connectivity

```bash
ping google.com
```

### Check Routing

```bash
ip route
```

### Capture Packets

```bash
tcpdump
```

---

## Cloud Relevance

Cloud networking heavily depends on:

- Layer 3 routing
- Layer 4 load balancing
- Layer 7 ingress

AWS ALB:
- Layer 7

AWS NLB:
- Layer 4

---

## Kubernetes Relevance

Kubernetes networking uses:

- Pod IPs
- Services
- kube-proxy
- DNS

OSI understanding helps debug:
- ingress issues
- service failures
- DNS problems

---

## Interview Questions

1. Difference between Layer 2 and Layer 3?
2. Which layer uses IP?
3. Which layer uses TCP?
4. What is encapsulation?
5. Difference between OSI and TCP/IP model?

---

## Summary

OSI model provides a structured way to understand and troubleshoot network communication.