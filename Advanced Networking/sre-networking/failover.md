# Failover Systems

## Overview

Failover is the process of switching to a backup system when the primary fails.

---

## Why It Matters

Ensures:
- continuous availability
- minimal downtime

---

## Types

### 1. Active-Passive
- one active system
- one standby

### 2. Active-Active
- both systems serve traffic

---

## Flow

Primary fails → Backup takes over → Traffic continues

---

## Real-world Example

Database failover:
- primary DB goes down
- replica becomes primary

---

## SRE Concerns

- failover speed
- data consistency
- health checks

---

## Troubleshooting

### Issue: failover not triggering

Check:
- health check config
- monitoring system

---

## Interview Questions

1. What is failover?
2. Active-active vs active-passive?
3. What triggers failover?

---

## Summary

Failover ensures system continuity during failures.