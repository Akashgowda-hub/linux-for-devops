# Ingress in Kubernetes

## Overview

Ingress manages external HTTP/HTTPS access to services.

---

## Why Ingress Matters

Without ingress:
- each service needs its own LoadBalancer
- cost increases
- management becomes complex

---

## Architecture

Internet → Ingress Controller → Service → Pod

---

## Ingress Controller

Examples:
- NGINX Ingress
- Traefik
- HAProxy

---

## Example Rule

```yaml
host: app.example.com
path: /
service: frontend
```

---

## TLS Support

Ingress supports HTTPS termination.

---

## Real-world Example

Single domain routes:

- /api → backend
- /web → frontend

---

## Troubleshooting

### Ingress not working

Check:
- controller running
- service endpoints
- DNS mapping

---

## Interview Questions

1. What is Ingress?
2. Why not use LoadBalancer directly?
3. What is ingress controller?

---

## Summary

Ingress provides smart HTTP routing for Kubernetes.