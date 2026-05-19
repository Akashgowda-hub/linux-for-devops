# TCP/IP Model

## Overview

TCP/IP is the practical networking model used on the Internet.

It is simpler than the OSI model.

---

## TCP/IP Layers

| Layer | Protocols |
|---|---|
| Application | HTTP, DNS, SSH |
| Transport | TCP, UDP |
| Internet | IP, ICMP |
| Network Access | Ethernet |

---

## Comparison with OSI

| OSI | TCP/IP |
|---|---|
| 7 Layers | 4 Layers |
| Theoretical | Practical |
| Academic | Internet Standard |

---

## Core Protocols

### TCP

Reliable communication.

### UDP

Fast communication.

### IP

Routing packets.

### ICMP

Diagnostics.

---

## Real-world Example

When opening a website:

- HTTP handles communication
- TCP ensures reliability
- IP handles routing
- Ethernet transmits data

---

## Packet Journey

Browser
→ TCP
→ IP
→ Router
→ Internet
→ Server

---

## Linux Commands

### Check Connections

```bash
ss -tulnp
```

### Ping

```bash
ping google.com
```

### Trace Route

```bash
traceroute google.com
```

---

## Kubernetes Relevance

Kubernetes networking is built on:
- TCP/IP
- overlay networking
- virtual interfaces

---

## Interview Questions

1. Difference between TCP/IP and OSI?
2. Which protocol handles routing?
3. Why is TCP reliable?

---

## Summary

TCP/IP powers the modern Internet and cloud infrastructure.