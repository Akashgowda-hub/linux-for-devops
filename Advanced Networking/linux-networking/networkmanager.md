# NetworkManager

## Overview

NetworkManager manages network connections in Linux systems.

---

## Why Important

Used in:
- servers
- desktops
- cloud VMs

---

## Check Status

```bash
systemctl status NetworkManager
```

---

## List Connections

```bash
nmcli connection show
```

---

## Enable/Disable Interface

```bash
nmcli networking on
nmcli networking off
```

---

## Real-world Example

Used in:
- WiFi setup
- static IP config
- VPN connections

---

## Troubleshooting

### Scenario: No Network

Check NetworkManager status.

---

## Interview Questions

1. What is NetworkManager?
2. Difference between nmcli and ip?

---

## Summary

NetworkManager simplifies Linux networking configuration.