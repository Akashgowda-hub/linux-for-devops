# HTTP and HTTPS

## Overview

HTTP is the protocol used for web communication.

HTTPS is secure HTTP using SSL/TLS encryption.

---

## HTTP

HTTP stands for:

HyperText Transfer Protocol

Default Port:

```text
80
```

HTTP is:
- stateless
- request-response based

---

## HTTPS

HTTPS adds:
- encryption
- authentication
- integrity

Default Port:

```text
443
```

HTTPS uses:
- SSL/TLS certificates

---

## Why HTTPS Matters

HTTPS protects:
- passwords
- banking data
- API communication
- authentication tokens

---

## HTTP Request Flow

Browser
→ DNS lookup
→ TCP handshake
→ HTTP request
→ Server response

---

## Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve data |
| POST | Send data |
| PUT | Update |
| DELETE | Remove |

---

## HTTP Status Codes

| Code | Meaning |
|---|---|
| 200 | Success |
| 301 | Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

---

## Example HTTP Request

```http
GET / HTTP/1.1
Host: example.com
```

---

## Linux Commands

### Curl

```bash
curl https://google.com
```

### Headers Only

```bash
curl -I https://google.com
```

### Verbose Output

```bash
curl -v https://google.com
```

---

## SSL/TLS Verification

```bash
openssl s_client -connect google.com:443
```

---

## Real-world DevOps Usage

HTTP/HTTPS used in:
- APIs
- Kubernetes ingress
- load balancers
- microservices
- reverse proxies

---

## Kubernetes Relevance

Ingress controllers manage:
- HTTP routing
- TLS termination

Examples:
- NGINX Ingress
- Traefik

---

## Troubleshooting Scenarios

### Scenario 1: SSL Certificate Error

Check:

```bash
openssl s_client -connect domain:443
```

Possible causes:
- expired cert
- invalid CA
- hostname mismatch

---

### Scenario 2: HTTP 502 Bad Gateway

Possible causes:
- backend service down
- ingress issue
- reverse proxy failure

---

### Scenario 3: Slow Website

Check:
- DNS
- TLS negotiation
- backend latency

Tools:
- curl
- tcpdump
- browser devtools

---

## Security Considerations

### TLS

Encrypts communication.

---

### HSTS

Forces HTTPS connections.

---

### Common Risks

- weak ciphers
- expired certificates
- MITM attacks

---

## Interview Questions

1. Difference between HTTP and HTTPS?
2. What is TLS?
3. What causes 502 error?
4. What is SSL termination?
5. Explain HTTP request flow.

---

## Summary

HTTP/HTTPS powers modern web applications, APIs, cloud services, and Kubernetes ingress traffic.