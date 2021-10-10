---
title: "PORT 3389 - RDP"
draft: false
images: []
description: "RDP Cheatsheet"
lead: "RDP Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 3389
toc: true
---
```
rdesktop -u username -p password -g 85% -r disk:share=/root/ 10.11.1.111
rdesktop -u guest -p guest 10.11.1.111 -g 94%
```
### RDP VULN
```
nmap -p 3389 --script=rdp-vuln-ms12-020.nse
```
### BRUTE FORCE
```
ncrack -vv --user Administrator -P /root/oscp/passwords.txt rdp://1
```