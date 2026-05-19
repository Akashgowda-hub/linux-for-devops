# TCP vs UDP

## Overview

TCP and UDP are transport layer protocols used for communication between systems.

Both work at:
- OSI Layer 4
- TCP/IP Transport Layer

---

## TCP (Transmission Control Protocol)

TCP is:
- connection-oriented
- reliable
- ordered

TCP guarantees:
- packet delivery
- correct sequence
- retransmission if packets fail

---

## TCP Features

| Feature | Description |
|---|---|
| Reliable | Yes |
| Ordered Delivery | Yes |
| Connection Oriented | Yes |
| Error Checking | Yes |
| Retransmission | Yes |

---

## TCP 3-Way Handshake

Before communication starts:

1. SYN
2. SYN-ACK
3. ACK

This establishes connection reliability.

---

## TCP Termination

Connection closes using:
- FIN
- ACK

---

## Common TCP Protocols

| Protocol | Port |
|---|---|
| HTTP | 80 |
| HTTPS | 443 |
| SSH | 22 |
| MySQL | 3306 |

---

## UDP (User Datagram Protocol)

UDP is:
- connectionless
- faster
- lightweight

UDP does NOT guarantee:
- delivery
- order
- retransmission

---

## UDP Features

| Feature | Description |
|---|---|
| Reliable | No |
| Ordered | No |
| Fast | Yes |
| Lightweight | Yes |

---

## Common UDP Protocols

| Protocol | Port |
|---|---|
| DNS | 53 |
| DHCP | 67/68 |
| NTP | 123 |
| Streaming | Various |

---

## TCP vs UDP Comparison

| Feature | TCP | UDP |
|---|---|---|
| Reliable | Yes | No |
| Fast | No | Yes |
| Connection | Required | Not Required |
| Overhead | High | Low |
| Ordering | Guaranteed | Not Guaranteed |

---

## Real-world Examples

### TCP Use Cases

Used when reliability matters:
- banking
- APIs
- SSH
- databases

---

### UDP Use Cases

Used when speed matters:
- gaming
- streaming
- VoIP
- DNS

---

## Linux Commands

### Show TCP Connections

```bash
ss -t
```

### Show UDP Connections

```bash
ss -u
```

### Show All Listening Ports

```bash
ss -tulnp
```

---

## Packet Capture Example

### Capture TCP

```bash
sudo tcpdump tcp
```

### Capture UDP

```bash
sudo tcpdump udp
```

---

## Kubernetes Relevance

Kubernetes services support:
- TCP
- UDP

Examples:
- CoreDNS → UDP
- APIs → TCP

---

## Troubleshooting Scenarios

### Scenario 1: TCP Connection Timeout

Possible causes:
- firewall block
- service down
- network routing issue

Tools:

```bash
telnet host port
curl
tcpdump
```

---

### Scenario 2: UDP Packet Loss

Symptoms:
- choppy audio
- DNS timeout

Possible causes:
- congestion
- packet drops

---

## Interview Questions

1. Why is TCP reliable?
2. Why does DNS use UDP?
3. Difference between TCP and UDP?
4. What is the TCP handshake?
5. Why is UDP faster?

---

## Summary

TCP provides reliability while UDP provides speed.

Understanding both is critical for:
- DevOps
- SRE
- Kubernetes
- Cloud networking