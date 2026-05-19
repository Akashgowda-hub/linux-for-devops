# Network Policies in Kubernetes

## Overview

Network Policies control traffic between pods.

---

## Why Needed

By default:
- all pods can communicate

This is insecure.

---

## What Network Policies Do

They define:
- allowed ingress traffic
- allowed egress traffic

---

## Example

Allow only frontend → backend:

---

## Real-world Use

Used for:
- microservice isolation
- security compliance
- zero trust architecture

---

## Flow

Pod → Policy → Allow/Deny → Destination Pod

---

## Troubleshooting

### Pod cannot connect

Check:
- policy rules
- labels
- namespace selectors

---

## Interview Questions

1. What are network policies?
2. Default Kubernetes networking behavior?
3. Ingress vs egress rules?

---

## Summary

Network policies secure pod-to-pod communication.