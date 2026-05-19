# Packet Analysis

## Overview

Packet analysis is the process of inspecting network packets to debug issues.

---

## What Is a Packet?

A packet contains:
- header (routing info)
- payload (data)

---

## Key Fields

| Field | Meaning |
|---|---|
| Source IP | sender |
| Destination IP | receiver |
| Protocol | TCP/UDP |
| TTL | hop limit |

---

## Tools

- tcpdump
- Wireshark
- tshark

---

## Real-world Example

API failure investigation:

Check:
- request reached server?
- response returned?
- packet dropped?

---

## Packet Flow

Client → Router → Load Balancer → Server

---

## Troubleshooting

### Scenario: Packet Loss

Check:
- network congestion
- firewall
- MTU issues

---

## Interview Questions

1. What is a packet?
2. What causes packet loss?
3. How do you analyze packets?

---

## Summary

Packet analysis is core to SRE debugging.