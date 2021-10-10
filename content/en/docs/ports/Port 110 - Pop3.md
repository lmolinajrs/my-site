---
title: "PORT 110 - Pop3"
draft: false
images: []
description: "POP Cheatsheet"
lead: "POP Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 110
toc: true
---

```
telnet 10.11.1.111
USER pelle@10.11.1.111
PASS admin
​
# or:
​
USER pelle
PASS admin
​
# List all emails
list
​
# Retrieve email number 5, for example
retr 9
```