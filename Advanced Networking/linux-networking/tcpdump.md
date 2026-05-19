# tcpdump (Packet Analyzer)

## Overview

tcpdump captures network packets in real time.

---

## Why Important

Used in:
- production debugging
- security analysis
- Kubernetes troubleshooting
- performance analysis

---

## Basic Capture

```bash
sudo tcpdump
```

---

## Capture Interface

```bash
sudo tcpdump -i eth0
```

---

## Filter by Port

```bash
sudo tcpdump port 80
```

---

## Capture DNS Traffic

```bash
sudo tcpdump port 53
```

---

## Save to File

```bash
sudo tcpdump -w capture.pcap
```

---

## Read File

```bash
tcpdump -r capture.pcap
```

---

## Real-world Example

Debug:
- API failing
- DNS issue
- packet loss

---

## Packet Flow Analysis

Used to inspect:
- TCP handshake
- retransmissions
- latency issues

---

## Troubleshooting

### Scenario 1: No Traffic

Check:
- interface
- firewall

---

### Scenario 2: Packet Loss

Analyze:
- retransmissions
- congestion

---

## Interview Questions

1. What is tcpdump?
2. How to capture packets?
3. Difference between tcpdump and Wireshark?

---

## Summary

tcpdump is the most powerful Linux packet debugging tool.