# Latency Debugging

## Overview

Latency is the time taken for a request to complete.

---

## Types of Latency

| Type | Meaning |
|---|---|
| Network latency | travel time |
| Application latency | processing time |
| DNS latency | resolution time |

---

## How to Measure

```bash
ping google.com
```

---

## Tools

- ping
- traceroute
- curl
- tcpdump

---

## Real-world Example

Slow API response:

Break down:
- DNS lookup
- TCP handshake
- server processing
- response transfer

---

## Common Causes

- slow DNS
- overloaded server
- network congestion
- bad routing

---

## Debug Flow

1. ping test
2. traceroute
3. curl timing
4. packet capture

---

## Interview Questions

1. What causes latency?
2. How do you debug latency issues?
3. Difference between latency and throughput?

---

## Summary

Latency debugging requires multi-layer analysis.