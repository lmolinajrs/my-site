---
title: "PORT 23 - Telnet"
draft: false
images: []
description: "Telnet Cheatsheet"
lead: "Telnet Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 23
toc: true
---

### TELNET ENUM
```
nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 10.11.1.111
smtp-user-enum -M VRFY -U /root/sectools/SecLists/Usernames/Names/names.txt -t 10.11.1.111
```
### Bruteforce password
```
patator telnet_login host=10.11.1.110 inputs='FILE0\nFILE1' 0=/root/Desktop/user.txt 1=/root/Desktop/pass.txt  persistent=0 prompt_re='Username: | Password:'
```
```
nc <ip> 25
nc -nvv 10.11.1.111 25
HELO foo<cr><lf>

telnet 10.11.1.111 25
VRFY root
```