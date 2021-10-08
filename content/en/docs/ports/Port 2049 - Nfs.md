---
title: "PORT 2049 - NFS"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 2049
toc: true
---

```
nmap -p 111,2049 --script nfs-ls,nfs-showmount

showmount -e 10.11.1.111

# If you find anything you can mount it like this:

mount 10.11.1.111:/ /tmp/NFS –o nolock
mount -t nfs 10.11.1.111:/ /tmp/NFS –o nolock
```