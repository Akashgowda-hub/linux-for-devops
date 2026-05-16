# Linux for Kubernetes

Kubernetes relies heavily on Linux features.

---

# Important Linux Concepts

- Namespaces
- cgroups
- Container Runtime
- Networking
- iptables

---

# Namespaces

Provide isolation for:

- Processes
- Network
- Users
- Mount points

---

# cgroups

Manage CPU and memory limits.

Check cgroups:

```bash
mount | grep cgroup
```

---

# Container Runtime

Examples:

- containerd
- CRI-O
- Docker

---

# iptables

Used for Kubernetes networking.

```bash
sudo iptables -L
```

---

# Network Interfaces

Check interfaces:

```bash
ip a
```

---

# Check Listening Ports

```bash
ss -tulpn
```

---

# Kernel Modules

Common Kubernetes modules:

```bash
lsmod
```

---

# Summary

Kubernetes depends on Linux kernel networking and container features.