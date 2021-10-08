---
title: "PORT 69 - UDP - TFTP"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 69
toc: true
---
```
tftp <ip>
```
* Vulns tftp in server 1.3, 1.4, 1.9, 2.1, and a few more.
* Same checks as FTP Port 21.
```
nmap -p69 --script=tftp-enum.nse 10.11.1.111
```