# CNI (Container Network Interface)

## Overview

CNI is responsible for pod networking in Kubernetes.

---

## Why CNI Exists

Kubernetes does NOT define networking implementation.

CNI plugins handle:
- IP assignment
- routing
- network connectivity

---

## Popular CNIs

| CNI | Features |
|---|---|
| Calico | Security + routing |
| Flannel | Simple overlay |
| Cilium | eBPF-based |

---

## How It Works

1. Pod created
2. Kubelet calls CNI
3. CNI assigns IP
4. Routes configured

---

## Real-world Example

Pod gets IP:
```text
10.244.1.5
```

CNI ensures connectivity.

---

## Troubleshooting

### Pod has no IP

Check:
- CNI plugin running
- daemonset status

```bash
kubectl get pods -n kube-system
```

---

## Interview Questions

1. What is CNI?
2. Is CNI required?
3. Difference between Calico and Flannel?

---

## Summary

CNI is the backbone of Kubernetes networking.