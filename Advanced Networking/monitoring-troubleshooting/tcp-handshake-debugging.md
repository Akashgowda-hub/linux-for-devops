# TCP Handshake Debugging

## Overview

TCP connection issues often break applications.

---

## 3-Way Handshake

1. SYN
2. SYN-ACK
3. ACK

---

## Common Failures

### 1. SYN Sent, No Response

- firewall block
- server down

---

### 2. SYN-ACK Missing

- port closed
- security group issue

---

### 3. Connection Reset

- application crash
- load balancer issue

---

## Tools

```bash
tcpdump
ss
telnet
```

---

## Real-world Example

API not responding due to:
- blocked port 443

---

## Interview Questions

1. Explain TCP handshake.
2. What is SYN flood?
3. Why connections fail?

---

## Summary

TCP handshake debugging is essential for production reliability.