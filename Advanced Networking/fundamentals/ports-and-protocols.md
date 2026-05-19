# Ports and Protocols

## Overview

Ports identify services running on systems.

Protocols define communication rules.

---

## Common Ports

| Port | Protocol | Service |
|---|---|---|
| 22 | TCP | SSH |
| 53 | UDP/TCP | DNS |
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |

---

## Port Ranges

| Range | Purpose |
|---|---|
| 0-1023 | Well-known |
| 1024-49151 | Registered |
| 49152-65535 | Ephemeral |

---

## Real-world Example

When opening HTTPS website:

Browser connects to:
- destination IP
- port 443

---

## Linux Commands

### Show Listening Ports

```bash
ss -tulnp
```

### Netstat

```bash
netstat -tulnp
```

### Check Open Port

```bash
nc -zv google.com 443
```

---

## DevOps Relevance

Ports are critical in:
- Docker
- Kubernetes
- Firewalls
- Load balancers

---

## Kubernetes Example

Service exposes:
- port
- targetPort
- nodePort

---

## Troubleshooting

### Connection Refused

Possible causes:
- service down
- firewall blocked
- wrong port

---

## Interview Questions

1. Difference between port and protocol?
2. Why does HTTPS use 443?
3. What is an ephemeral port?

---

## Summary

Ports allow multiple services to communicate simultaneously on the same machine.