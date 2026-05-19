# Ping and Traceroute

## Overview

Used to test network connectivity and path.

---

## Ping

Tests:
- connectivity
- latency

```bash
ping google.com
```

---

## How Ping Works

Uses:
- ICMP Echo Request
- ICMP Echo Reply

---

## Traceroute

Shows path to destination.

```bash
traceroute google.com
```

---

## Why Important

Used in:
- DevOps debugging
- cloud networking issues
- Kubernetes troubleshooting

---

## Real-world Example

If website is slow:

- ping → latency check
- traceroute → where delay happens

---

## Packet Flow

Laptop → Router → ISP → Internet → Server

Each hop is shown in traceroute.

---

## Troubleshooting

### Scenario 1: No Response

Possible causes:
- firewall
- host down

---

### Scenario 2: High Latency

Find slow hop using traceroute.

---

## Interview Questions

1. What is ping?
2. What is traceroute?
3. Difference between them?

---

## Summary

Ping checks reachability; traceroute shows path.