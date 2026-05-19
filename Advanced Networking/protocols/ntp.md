# NTP (Network Time Protocol)

## Overview

NTP synchronizes system clocks across networks.

Accurate time is critical for:
- logs
- authentication
- distributed systems
- Kubernetes
- databases

---

## Why Time Synchronization Matters

Incorrect time can cause:
- SSL certificate failures
- authentication issues
- broken logs
- Kubernetes failures

---

## NTP Port

| Protocol | Port |
|---|---|
| UDP | 123 |

---

## Linux Commands

### Check Time

```bash
timedatectl
```

---

### Sync Time

```bash
sudo ntpdate pool.ntp.org
```

---

### Chrony Status

```bash
chronyc sources
```

---

## Real-world Example

Distributed systems require synchronized clocks for:
- replication
- logging
- transactions

---

## Kubernetes Relevance

Kubernetes nodes require:
- synchronized time
- accurate certificates

---

## Troubleshooting

### Scenario 1: Time Drift

Symptoms:
- SSL failures
- token expiration

Check:

```bash
timedatectl
```

---

### Scenario 2: NTP Not Syncing

Possible causes:
- firewall
- wrong NTP server
- UDP 123 blocked

---

## Security Considerations

- restrict public NTP
- avoid amplification abuse

---

## Interview Questions

1. What is NTP?
2. Why is time sync important?
3. What problems occur with wrong system time?

---

## Summary

NTP ensures accurate time synchronization for reliable distributed systems.