---
title: "PORT 389 - 636 - Ldap"
draft: false
images: []
description: "LDAP Cheatsheet"
lead: "LDAP Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 389
toc: true
---
```
ldapsearch -h 10.11.1.111 -p 389 -x -b "dc=mywebsite,dc=com"
ldapsearch -x -h 10.11.1.111 -D 'DOMAIN\user' -w 'hash-password'
ldapdomaindump 10.11.1.111 -u 'DOMAIN\user' -p 'hash-password'
```
### BRUTE FORCE
```
patator ldap_login host=10.10.1.111 1=/root/Downloads/passwords_ssh.txt user=hsmith password=FILE1 -x ignore:mesg='Authentication failed.'
```