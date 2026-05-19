# Wireshark (Network Packet Analyzer)

## Overview

Wireshark is a GUI tool used to inspect network packets in real time.

It helps you see:
- TCP handshake
- DNS queries
- HTTP requests
- packet loss
- retransmissions

---

## Why It Matters

Used in:
- production debugging
- security analysis
- performance tuning
- incident response

---

## What You Can See

- source IP
- destination IP
- protocols
- payload data
- timing

---

## Basic Filters

### HTTP traffic

```text
http
```

### DNS traffic

```text
dns
```

### TCP traffic

```text
tcp
```

---

## Real-world Example

Debugging slow API:

You can see:
- request delay
- server response time
- retransmissions

---

## TCP Stream

Follow full connection:

```text
Follow → TCP Stream
```

---

## Troubleshooting Scenarios

### Scenario 1: Slow Website

Check:
- handshake delay
- packet retransmissions

---

### Scenario 2: DNS Failure

Check:
- DNS request sent?
- response received?

---

## Interview Questions

1. What is Wireshark used for?
2. Can it capture encrypted traffic?
3. Difference between tcpdump and Wireshark?

---

## Summary

Wireshark gives full visibility into network behavior.