# Reverse Proxy

## Overview

A reverse proxy sits between users and backend servers.

---

## Why It Matters

It provides:
- security
- load distribution
- SSL termination
- caching

---

## Architecture

Client → Reverse Proxy → Backend Server

---

## Examples

- NGINX
- HAProxy
- Traefik

---

## Use Cases

- API gateway
- web server routing
- microservices entry point

---

## Key Features

- hides backend servers
- improves security
- handles SSL/TLS

---

## Real-world Example

NGINX routes:

- /api → backend
- /web → frontend

---

## Troubleshooting

### Issue: backend not reachable

Check:
- proxy config
- upstream server health

---

## Interview Questions

1. What is reverse proxy?
2. Difference between proxy and reverse proxy?
3. Why use NGINX?

---

## Summary

Reverse proxy is the entry point for most modern web systems.