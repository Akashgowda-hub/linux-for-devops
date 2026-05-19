# ARP (Address Resolution Protocol)

## Overview

ARP maps IP addresses to MAC addresses inside local networks.

Computers use ARP to discover the physical address of another device.

---

## Why ARP Matters

ARP is required because:
- Ethernet communication uses MAC addresses
- applications use IP addresses

ARP bridges Layer 3 and Layer 2.

---

## How ARP Works

Suppose:

```text
Host A:
192.168.1.10
```

wants to communicate with:

```text
192.168.1.20
```

Steps:

1. Host checks ARP cache
2. If MAC unknown:
   - sends ARP broadcast
3. Destination replies with MAC address
4. MAC stored in ARP cache

---

## ARP Request

Broadcast:

```text
Who has 192.168.1.20?
```

---

## ARP Reply

```text
192.168.1.20 is at 00:1A:2B:3C:4D:5E
```

---

## ARP Cache

Stored locally for faster communication.

---

## Linux Commands

### Show ARP Cache

```bash
arp -a
```

OR

```bash
ip neigh
```

---

### Clear ARP Cache

```bash
sudo ip neigh flush all
```

---

## Real-world Example

Laptop connecting to home router:

- laptop knows router IP
- ARP discovers router MAC
- packets forwarded

---

## Packet Flow

IP Packet
→ ARP lookup
→ MAC discovered
→ Ethernet frame created

---

## Kubernetes Relevance

Kubernetes nodes use ARP for:
- local network communication
- node communication
- overlay networking

---

## Security Risks

### ARP Spoofing

Attacker sends fake ARP replies.

Can cause:
- MITM attacks
- traffic interception

---

## Troubleshooting Scenarios

### Scenario 1: Duplicate IP

Symptoms:
- intermittent network failure

Check:

```bash
arp -a
```

---

### Scenario 2: Cannot Reach Local Device

Possible causes:
- stale ARP cache
- switch issue
- wrong subnet

---

## Interview Questions

1. What is ARP?
2. Why is ARP needed?
3. Difference between ARP and DNS?
4. What is ARP spoofing?

---

## Summary

ARP enables local network communication by mapping IP addresses to MAC addresses.