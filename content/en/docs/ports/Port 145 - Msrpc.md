---
title: "PORT 145 - Msrpc"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 145
toc: true
---

```
nmap 10.11.1.111 --script=msrpc-enum
msf > use exploit/windows/dcerpc/ms03_026_dcom

# Endpoint Mapper Service Discovery
use auxiliary/scanner/dcerpc/endpoint_mapper

#Hidden DCERPC Service Discovery
use auxiliary/scanner/dcerpc/hidden

# Remote Management Interface Discovery
use auxiliary/scanner/dcerpc/management

# DCERPC TCP Service Auditor
use auxiliary/scanner/dcerpc/tcp_dcerpc_auditor

impacket-rpcdump

# Enum network interface
# https://github.com/mubix/IOXIDResolver
```