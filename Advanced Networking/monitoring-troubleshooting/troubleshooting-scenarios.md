# Real-world Troubleshooting Scenarios

## Overview

This file combines everything into production-like problems.

---

## Scenario 1: Website Down

Check:
- DNS
- load balancer
- backend service

---

## Scenario 2: API Timeout

Check:
- latency
- TCP handshake
- server logs

---

## Scenario 3: Kubernetes Service Down

Check:
- pods
- endpoints
- CoreDNS

---

## Scenario 4: High Latency

Check:
- network hops
- packet loss
- DNS resolution

---

## Scenario 5: Intermittent Failure

Check:
- network policies
- firewall rules
- routing tables

---

## Debug Flow

1. ping
2. traceroute
3. curl
4. tcpdump
5. logs

---

## Interview Questions

1. How do you debug production issues?
2. What is your step-by-step approach?
3. Which tools do you use first?

---

## Summary

Real engineering is about structured debugging, not guessing.