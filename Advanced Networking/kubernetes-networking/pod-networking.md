# Pod Networking in Kubernetes

## Overview

In Kubernetes, every Pod gets its own IP address.

This means:
- no NAT inside cluster
- pods communicate directly
- flat networking model

---

## Why Pod Networking Matters

It enables:
- microservices communication
- service discovery
- distributed systems

---

## Key Rule

> Every Pod must be able to communicate with every other Pod.

Across:
- nodes
- namespaces
- services

---

## Architecture

Node A
  ├── Pod 1 (10.244.1.2)
  └── Pod 2 (10.244.1.3)

Node B
  ├── Pod 3 (10.244.2.4)

All pods can talk directly.

---

## How It Works

Pod networking is NOT native to Kubernetes.

It is implemented by:
- CNI plugins (Calico, Flannel, Cilium)

---

## Real-world Flow

Pod A → Pod B

Steps:
1. Pod A sends packet
2. CNI routes it
3. Node networking handles forwarding
4. Pod B receives packet

---

## Linux Commands

```bash
ip addr
ip route
```

Check pod interfaces:

```bash
kubectl exec -it pod -- ip addr
```

---

## Troubleshooting

### Pod cannot reach another pod

Check:
- CNI plugin
- routing table
- security rules

---

## Interview Questions

1. Do pods get IPs?
2. Who provides pod networking?
3. Can pods communicate across nodes?

---

## Summary

Pod networking is the foundation of Kubernetes communication.