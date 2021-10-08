---
title: "PORT 139 - 445 - Smb"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 139
toc: true
---

### SMB ENUM
```
nmap --script smb-enum-*,smb-vuln-*,smb-ls.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-print-text.nse,smb-psexec.nse,smb-security-mode.nse,smb-server-stats.nse,smb-system-info.nse,smb-protocols -p 139,445 10.11.1.111

nmap --script smb-enum-domains.nse,smb-enum-groups.nse,smb-enum-processes.nse,smb-enum-sessions.nse,smb-enum-shares.nse,smb-enum-users.nse,smb-ls.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-print-text.nse,smb-psexec.nse,smb-security-mode.nse,smb-server-stats.nse,smb-system-info.nse,smb-vuln-conficker.nse,smb-vuln-cve2009-3103.nse,smb-vuln-ms06-025.nse,smb-vuln-ms07-029.nse,smb-vuln-ms08-067.nse,smb-vuln-ms10-054.nse,smb-vuln-ms10-061.nse,smb-vuln-regsvc-dos.nse -p 139,445 10.11.1.111

enum4linux -a 10.11.1.111

msf5 > use auxiliary/scanner/smb/smb_login

msf5 > use auxiliary/scanner/smb/smb_enumusers
```
### RPC
```
rpcclient -U "" 10.11.1.111
    srvinfo
    enumdomusers
    getdompwinfo
    querydominfo
    netshareenum
    netshareenumall
```
### SMBCLIENT
```
smbclient -L 10.11.1.111
smbclient -N -L //10.11.1.111
smbclient //10.11.1.111/tmp
smbclient \\\\10.11.1.111\\ipc$ -U john
smbclient //10.11.1.111/ipc$ -U john

################
RECURSIVE ENUM
################

smb: \> recurse on                                                                                                                   
smb: \> ls  
```
### DOWNLOAD ALL
```
smbget -R  smb://10.10.10.178/Data/ -U TempUser
```