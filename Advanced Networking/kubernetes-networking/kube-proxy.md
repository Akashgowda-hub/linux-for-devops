# kube-proxy

## Overview

kube-proxy manages Kubernetes Service networking.

---

## Role

It handles:
- service IPs
- traffic routing
- load balancing

---

## Modes

| Mode | Description |
|---|---|
| iptables | default |
| IPVS | high performance |

---

## Flow

Service → kube-proxy → Pod

---

## Real-world Example

Traffic to:

```text
10.96.0.10 (ClusterIP)
```

is forwarded to pods.

---

## Troubleshooting

### Service not routing

Check:
- kube-proxy logs
- endpoints

---

## Interview Questions

1. What is kube-proxy?
2. iptables vs IPVS?
3. How services work internally?

---

## Summary

kube-proxy enables service networking in Kubernetes.