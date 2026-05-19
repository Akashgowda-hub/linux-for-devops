# FTP and SFTP

## Overview

FTP and SFTP are protocols used for file transfer between systems.

---

## FTP (File Transfer Protocol)

FTP is an older file transfer protocol.

Default Ports:

| Port | Purpose |
|---|---|
| 21 | Control |
| 20 | Data |

FTP is NOT encrypted.

---

## Problems with FTP

FTP sends:
- usernames
- passwords
- data

in plain text.

This makes it insecure.

---

## SFTP (SSH File Transfer Protocol)

SFTP is secure file transfer over SSH.

Default Port:

```text
22
```

SFTP encrypts:
- authentication
- commands
- file transfers

---

## FTP vs SFTP

| Feature | FTP | SFTP |
|---|---|---|
| Encryption | No | Yes |
| Secure | No | Yes |
| Port | 21 | 22 |
| Uses SSH | No | Yes |

---

## Linux Commands

### FTP Connection

```bash
ftp server-ip
```

---

### SFTP Connection

```bash
sftp user@server-ip
```

---

### Upload File

```bash
put file.txt
```

---

### Download File

```bash
get file.txt
```

---

## SCP Alternative

```bash
scp file.txt user@server:/tmp
```

---

## Real-world Usage

SFTP commonly used for:
- backup transfers
- automation
- enterprise file exchange
- CI/CD artifacts

---

## DevOps Relevance

Used in:
- deployment pipelines
- log transfer
- backup systems

---

## Troubleshooting

### Scenario 1: Authentication Failure

Possible causes:
- wrong credentials
- SSH issue
- permission problem

---

### Scenario 2: Connection Timeout

Check:
- firewall
- SSH service
- network routing

---

## Security Best Practices

- avoid FTP
- use SFTP
- use SSH keys
- restrict access

---

## Interview Questions

1. Difference between FTP and SFTP?
2. Why is FTP insecure?
3. Does SFTP use SSH?

---

## Summary

SFTP provides secure encrypted file transfer and is preferred over FTP in modern infrastructure.