# ICMP (Internet Control Message Protocol)

## Overview

ICMP is used for:
- diagnostics
- error reporting
- network troubleshooting

ICMP works at:
- OSI Layer 3

---

## Why ICMP Matters

ICMP helps engineers:
- test connectivity
- identify routing issues
- debug packet loss

---

## Common ICMP Messages

| Type | Purpose |
|---|---|
| Echo Request | Ping request |
| Echo Reply | Ping response |
| Destination Unreachable | Routing issue |
| Time Exceeded | TTL expired |

---

## Ping

### Basic Ping

```bash
ping google.com
```

Used to:
- test connectivity
- measure latency

---

## Traceroute

```bash
traceroute google.com
```

Shows:
- packet path
- network hops

---

## TTL (Time To Live)

TTL prevents infinite routing loops.

Each router:
- decreases TTL
- drops packet at zero

---

## Real-world Example

If a server is unreachable:

Use:
- ping
- traceroute

to identify failure point.

---

## Kubernetes Relevance

ICMP helps debug:
- node communication
- pod connectivity
- network latency

---

## Troubleshooting

### Scenario 1: Ping Fails

Possible causes:
- firewall
- routing issue
- host down

---

### Scenario 2: High Latency

Use:

```bash
traceroute
```

Possible causes:
- congestion
- ISP routing
- overloaded network

---

## Security Considerations

Some environments block ICMP to reduce:
- scanning
- reconnaissance

---

## Interview Questions

1. What is ICMP?
2. How does ping work?
3. What is TTL?
4. Why does traceroute work?

---

## Summary

ICMP is essential for diagnosing and troubleshooting network connectivity problems.