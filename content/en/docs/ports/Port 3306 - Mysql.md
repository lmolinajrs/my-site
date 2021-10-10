---
title: "PORT 3306 - MYSQL"
draft: false
images: []
description: "MYSQL Cheatsheet"
lead: "MYSQL Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 3306
toc: true
---
```
mysql --host=10.11.1.111 -u root -p
```
### MYSQL ENUM
```
nmap --script=mysql-databases.nse,mysql-empty-password.nse,mysql-enum.nse,mysql-info.nse,mysql-variables.nse,mysql-vuln-cve2012-2122.nse 10.11.1.111 -p 3306
```
### COMMANDS
* mysql -u root -p
* show databases;
* use database_name;
* show tables;
* show columns from table_name
* select user, password from table_name