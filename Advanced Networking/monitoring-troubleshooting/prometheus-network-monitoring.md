# Prometheus Network Monitoring

## Overview

Prometheus is a monitoring system used to collect metrics.

---

## Why It Matters

Used in:
- Kubernetes monitoring
- SRE dashboards
- alerting systems

---

## Key Metrics

- latency
- packet loss
- error rates
- throughput

---

## Architecture

Exporter → Prometheus → Grafana

---

## Real-world Example

Monitor:
- API latency spikes
- node network errors

---

## PromQL Example

```text
rate(http_requests_total[5m])
```

---

## Alerts

- high latency
- service down
- DNS failure

---

## Interview Questions

1. What is Prometheus?
2. What is PromQL?
3. Why monitoring is important?

---

## Summary

Prometheus provides observability for network and system health.