---
title: "PORT 5432 - PostgreSQL"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 5432
toc: true
---

```
psql -h 10.10.1.111 -U postgres -W

# Default creds
postgres : postgres
postgres : password
postgres : admin
admin : admin
admin : password

pg_dump --host=10.10.1.111 --username=postgres --password --dbname=template1 --table='users' -f output_pgdump
```