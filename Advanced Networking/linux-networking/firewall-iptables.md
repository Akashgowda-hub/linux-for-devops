# iptables Firewall

## Overview

iptables controls network traffic rules.

---

## Why Important

Used for:
- security
- traffic filtering
- cloud firewall logic

---

## List Rules

```bash
sudo iptables -L
```

---

## Allow Port

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

---

## Block Port

```bash
sudo iptables -A INPUT -p tcp --dport 80 -j DROP
```

---

## Real-world Example

Block malicious traffic to server.

---

## Troubleshooting

Check firewall rules if:
- service not reachable

---

## Interview Questions

1. What is iptables?
2. Difference between ACCEPT and DROP?

---

## Summary

iptables controls Linux network security.