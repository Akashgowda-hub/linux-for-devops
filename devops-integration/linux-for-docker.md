# Linux for Docker

Docker uses Linux kernel features for containerization.

---

# Important Linux Concepts

- Namespaces
- cgroups
- OverlayFS
- Virtual Networking

---

# Docker Installation

Ubuntu example:

```bash
sudo apt update
sudo apt install docker.io
```

---

# Docker Commands

## docker ps

Show running containers.

```bash
docker ps
```

---

## docker images

List images.

```bash
docker images
```

---

## docker run

Run container.

```bash
docker run nginx
```

---

# cgroups

Control CPU and memory usage.

Check cgroups:

```bash
mount | grep cgroup
```

---

# Namespaces

Provide isolation for:

- Processes
- Network
- Users
- Filesystems

---

# Docker Networking

List docker networks.

```bash
docker network ls
```

---

# Summary

Docker depends heavily on Linux kernel capabilities.