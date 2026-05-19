# SMTP (Simple Mail Transfer Protocol)

## Overview

SMTP is the protocol used for sending emails.

SMTP handles:
- email transmission
- mail relaying
- outbound email delivery

---

## SMTP Ports

| Port | Purpose |
|---|---|
| 25 | Traditional SMTP |
| 587 | Secure Submission |
| 465 | SMTPS |

---

## Email Flow

Sender
→ SMTP Server
→ Recipient Mail Server
→ Inbox

---

## Related Protocols

| Protocol | Purpose |
|---|---|
| SMTP | Send email |
| IMAP | Read email |
| POP3 | Download email |

---

## Real-world Example

Application sends:
- password reset
- alerts
- notifications

through SMTP server.

---

## Linux Commands

### Test SMTP

```bash
telnet smtp.server.com 25
```

---

### Send Email (mail command)

```bash
mail -s "Test" user@example.com
```

---

## DNS and SMTP

SMTP depends heavily on:
- MX records
- reverse DNS
- SPF/DKIM/DMARC

---

## Cloud Relevance

Cloud providers:
- AWS SES
- SendGrid
- Mailgun

---

## Troubleshooting

### Scenario 1: Email Not Delivered

Check:
- MX records
- spam filtering
- SMTP logs

---

### Scenario 2: Port 25 Blocked

Cloud providers often block:
- outbound SMTP

Use:
- port 587
- mail relay service

---

## Security

### SPF

Prevents spoofing.

---

### DKIM

Signs emails cryptographically.

---

### DMARC

Protects domain reputation.

---

## Interview Questions

1. What is SMTP?
2. Difference between SMTP and IMAP?
3. Why is port 587 preferred?

---

## Summary

SMTP powers email delivery systems across the Internet.