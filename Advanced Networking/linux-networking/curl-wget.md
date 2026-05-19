# curl and wget

## Overview

Used to fetch data from URLs.

---

## curl

Flexible HTTP client.

```bash
curl https://example.com
```

---

## wget

Used for downloading files.

```bash
wget https://example.com/file.zip
```

---

## Check Headers

```bash
curl -I https://google.com
```

---

## POST Request

```bash
curl -X POST https://api.example.com
```

---

## Real-world Usage

Used in:
- APIs
- CI/CD pipelines
- health checks

---

## Troubleshooting

### Scenario 1: API Fails

Check:
- response code
- headers
- TLS errors

---

## Interview Questions

1. Difference between curl and wget?
2. What is HTTP GET vs POST?

---

## Summary

curl is essential for API debugging.