---
title: "PORT 5985 - Winrm"
draft: false
images: []
description: "WinRM Cheatsheet"
lead: "WinRM Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 5985
toc: true
---
```
#gem install evil-winrm
evil-winrm -i 10.11.1.111 -u Administrator -p 'password1'
evil-winrm -i 10.11.1.111 -u Administrator -H 'hash-pass' -s /scripts/folder
```