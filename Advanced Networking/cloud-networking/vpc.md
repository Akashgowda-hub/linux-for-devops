# SSH Lab

## Objective

Practice SSH in real scenarios.

---

## Setup

Two machines:
- local
- remote server

---

## Connect

```bash
ssh user@ip
```

---

## Key-based Login

```bash
ssh-keygen
ssh-copy-id user@server
```

---

## Secure Config

Disable password login:

```text
PasswordAuthentication no
```

---

## Troubleshooting

- permission denied
- timeout
- host unreachable

---

## Real-world Use

Used in:
- DevOps automation
- CI/CD pipelines
- Kubernetes nodes

---

## Summary

SSH is core for infrastructure access.