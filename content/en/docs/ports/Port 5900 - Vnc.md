---
title: "PORT 5900 - VNC"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 5900
toc: true
---

```
nmap --script=vnc-info,vnc-brute,vnc-title -p 5900 10.11.1.111
```
Check poison HTB for privilege escalation with VNC