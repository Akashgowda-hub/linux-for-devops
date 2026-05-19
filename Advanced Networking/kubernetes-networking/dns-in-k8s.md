# DNS in Kubernetes

## Overview

Kubernetes uses CoreDNS for service discovery.

---

## Why DNS Matters

Pods communicate using:
```text
service-name.namespace.svc.cluster.local
```

---

## CoreDNS Role

- resolves service names
- maps to ClusterIP
- enables microservices communication

---

## Example

```text
backend.default.svc.cluster.local → 10.96.0.5
```

---

## Flow

Pod → CoreDNS → Service IP → Pod

---

## Troubleshooting

### DNS not resolving

Check:

```bash
kubectl get pods -n kube-system
kubectl logs coredns
```

---

## Interview Questions

1. What is CoreDNS?
2. How does service discovery work?
3. Why DNS is critical in K8s?

---

## Summary

DNS enables service-to-service communication in Kubernetes.