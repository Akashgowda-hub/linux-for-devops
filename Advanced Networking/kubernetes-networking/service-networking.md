# Service Networking in Kubernetes

## Overview

A Service provides a stable IP and DNS name for pods.

Because:
- pods are ephemeral
- pod IPs change

---

## Why Services Exist

To solve:
- unstable pod IPs
- load balancing
- service discovery

---

## Types of Services

| Type | Purpose |
|---|---|
| ClusterIP | Internal only |
| NodePort | Expose via node |
| LoadBalancer | Cloud LB |
| ExternalName | DNS mapping |

---

## Architecture

Client → Service → Pod

---

## ClusterIP Example

```text
10.96.0.10 → Service IP
```

---

## DNS Name

```text
my-service.default.svc.cluster.local
```

---

## kube-proxy Role

Service traffic is handled by:
- kube-proxy
- iptables or ipvs

---

## Real-world Example

Frontend service connects to backend service using ClusterIP.

---

## Troubleshooting

### Service not reachable

Check:
- endpoints
- selectors
- pod labels

```bash
kubectl get endpoints
```

---

## Interview Questions

1. Why do we need services?
2. Difference between ClusterIP and NodePort?
3. What is kube-proxy?

---

## Summary

Services provide stable networking abstraction in Kubernetes.