# High Availability (HA)

## Overview

High availability ensures systems stay operational even when components fail.

---

## Goal

- minimize downtime
- ensure redundancy
- automatic recovery

---

## Architecture

Multi-zone / Multi-region systems:

```text
Region A → Region B (backup)
```

---

## Key Concepts

| Concept | Meaning |
|---|---|
| Redundancy | duplicate systems |
| Failover | switch to backup |
| Health check | system monitoring |

---

## Real-world Example

If one server fails:
→ traffic shifts to another

---

## SRE Focus

- uptime (99.9%+)
- fault tolerance
- disaster recovery

---

## Troubleshooting

### Issue: single point of failure

Fix:
- add redundancy
- load balancing

---

## Interview Questions

1. What is high availability?
2. What is failover?
3. Difference between HA and scalability?

---

## Summary

HA ensures systems survive failures without downtime.