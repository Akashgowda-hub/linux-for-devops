# netstat vs ss

## Overview

Both tools show network connections.

- `netstat` → older
- `ss` → modern, faster

---

## Why Important

Used for:
- debugging services
- checking ports
- identifying connections
- production troubleshooting

---

## Show Listening Ports

### ss (recommended)

```bash
ss -tulnp
```

---

### netstat (legacy)

```bash
netstat -tulnp
```

---

## Understand Flags

| Flag | Meaning |
|---|---|
| t | TCP |
| u | UDP |
| l | listening |
| n | numeric |
| p | process |

---

## Show Active Connections

```bash
ss -tn
```

---

## Show Process Using Port

```bash
ss -tulnp | grep 80
```

---

## Real-world Example

Debugging:
- nginx not responding
- backend service down
- port conflicts

---

## Troubleshooting

### Scenario 1: Port Not Listening

```bash
ss -tulnp
```

If missing:
- service not running

---

### Scenario 2: Port Conflict

Two services using same port → failure

---

## Interview Questions

1. Difference between netstat and ss?
2. How to check open ports?
3. How to find process using port?

---

## Summary

`ss` is the modern tool for network debugging in Linux.