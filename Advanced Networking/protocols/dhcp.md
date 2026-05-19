# DHCP

## Overview

DHCP dynamically assigns IP addresses to devices.

DHCP stands for:

Dynamic Host Configuration Protocol

---

## Why DHCP Matters

Without DHCP:
- manual IP assignment required
- large networks become difficult to manage

DHCP automatically provides:
- IP address
- subnet mask
- gateway
- DNS server

---

## DHCP Process (DORA)

| Step | Meaning |
|---|---|
| Discover | Client searches DHCP server |
| Offer | Server offers IP |
| Request | Client requests IP |
| Acknowledge | Server confirms |

---

## DHCP Ports

| Protocol | Port |
|---|---|
| UDP | 67 |
| UDP | 68 |

---

## Linux Commands

### Show IP

```bash
ip addr
```

### Renew DHCP Lease

```bash
sudo dhclient
```

---

## Real-world Example

Laptop joins WiFi:
- router assigns IP automatically

---

## Cloud Relevance

Cloud providers use DHCP internally for:
- VM networking
- metadata services

---

## Troubleshooting

### No IP Assigned

Check:
- DHCP server
- switch VLAN
- cable

---

### IP Conflict

Possible causes:
- duplicate static IP
- stale lease

---

## Interview Questions

1. What is DHCP?
2. Explain DORA process.
3. Why does DHCP use UDP?

---

## Summary

DHCP automates IP address management in modern networks.