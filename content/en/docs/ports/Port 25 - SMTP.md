---
title: "PORT 25 - SMTP"
draft: false
images: []
description: "SMPT Cheatsheet"
lead: "SMTP Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 25
toc: true
---

```nc -nvv 10.11.1.111 25
HELO foo

telnet 10.11.1.111 25
VRFY root

nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 10.11.1.111
smtp-user-enum -M VRFY -U /root/sectools/SecLists/Usernames/Names/names.txt -t 10.11.1.111

# SMTP relay
msfconsole
use auxiliary/scanner/smtp/smtp_relay
set RHOSTS <IP or File>
set MAILFROM <PoC email address>
set MAILTO <your email address>
run

# Send email unauth:

MAIL FROM:admin@admin.com
RCPT TO:DestinationEmail@DestinationDomain.com
DATA
test

.

Receive:
250 OK
```