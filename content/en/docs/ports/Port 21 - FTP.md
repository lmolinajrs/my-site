---
title: "PORT 21 - FTP"
draft: false
images: []
description: "FTP Cheatsheet"
lead: "FTP Cheatsheet"
menu:
  docs:
    parent: "Ports"
weight: 21
toc: true
---

## Enumeration
### Banner Grabbing
```
nc -vn <IP> 21
openssl s_client -connect crossfit.htb:21 -starttls ftp #Get certificate if any
```
### Connect to FTP using starttls
```
lftp
lftp :~> set ftp:ssl-force true
lftp :~> set ssl:verify-certificate no
lftp :~> connect 10.10.10.208
lftp 10.10.10.208:~> login                       
Usage: login <user|URL> [<pass>]
lftp 10.10.10.208:~> login username Password
```
### Unauth enum
You can us the commands HELP and FEAT to obtain some information of the FTP server:
```
HELP
214-The following commands are recognized (* =>'s unimplemented):
214-CWD     XCWD    CDUP    XCUP    SMNT*   QUIT    PORT    PASV    
214-EPRT    EPSV    ALLO*   RNFR    RNTO    DELE    MDTM    RMD     
214-XRMD    MKD     XMKD    PWD     XPWD    SIZE    SYST    HELP    
214-NOOP    FEAT    OPTS    AUTH    CCC*    CONF*   ENC*    MIC*    
214-PBSZ    PROT    TYPE    STRU    MODE    RETR    STOR    STOU    
214-APPE    REST    ABOR    USER    PASS    ACCT*   REIN*   LIST    
214-NLST    STAT    SITE    MLSD    MLST    
214 Direct comments to root@drei.work
FEAT
211-Features:
 PROT
 CCC
 PBSZ
 AUTH TLS
 MFF modify;UNIX.group;UNIX.mode;
 REST STREAM
 MLST modify*;perm*;size*;type*;unique*;UNIX.group*;UNIX.mode*;UNIX.owner*;
 UTF8
 EPRT
 EPSV
 LANG en-US
 MDTM
 SSCN
 TVFS
 MFMT
 SIZE
211 End
```
### Connections
In Active FTP the FTP client first initiates the control connection from its port N to FTP Servers command port – port 21. The client then listens to port N+1 and sends the port N+1 to FTP Server. FTP Server then initiates the data connection, from its port M to the port N+1 of the FTP Client.

But, if the FTP Client has a firewall setup that controls the incoming data connections from outside, then active FTP may be a problem. And, a feasible solution for that is Passive FTP.

In Passive FTP, the client initiates the control connection from its port N to the port 21 of FTP Server. After this, the client issues a passv comand. The server then sends the client one of its port number M. And the client initiates the data connection from its port P to port M of the FTP Server.

### Anonymous login
```
ftp <IP>
>anonymous
>anonymous
>ls -a # List all files (even hidden) (yes, they could be hidden)
>binary #Set transmission to binary instead of ascii
>ascii #Set transmission to ascii instead of binary
>bye #exit
```
### Brute force
Here you can find a nice list with default ftp credentials: https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt
### Automated
Anon login and bounce FTP checks are perform by default by nmap with -sC option or:
```
nmap --script ftp-* -p 21 <ip>
```
### Shodan
```
ftp
port:21
```
### Browser connection
You can connect to a FTP server using a browser (like Firefox) using a URL like:
```
ftp://anonymous:anonymous@10.10.10.98
```
Note that if a web application is sending data controlled by a user directly to a FTP server you can send double URL encode %0d%0a (in double URL encode this is %250d%250a) bytes and make the FTP server perform arbitrary actions. One of this possible arbitrary actions is to download content from a users controlled server, perform port scanning or try to talk to other plain-text based services (like http).
### Download all files from FTP
```
wget -m ftp://anonymous:anonymous@10.10.10.98 #Donwload all
wget -m --no-passive ftp://anonymous:anonymous@10.10.10.98 #Download all
```
### Some FTP commands
```
USER username
PASS password
HELP The server indicates which commands are supported
PORT 127,0,0,1,0,80This will indicate the FTP server to establish a connection with the IP 127.0.0.1 in port 80 (you need to put the 5th char as "0" and the 6th as the port in decimal or use the 5th and 6th to express the port in hex).
EPRT |2|127.0.0.1|80|This will indicate the FTP server to establish a TCP connection (indicated by "2") with the IP 127.0.0.1 in port 80. This command supports IPv6.
LIST This will send the list of files in current folder
APPE /path/something.txt This will indicate the FTP to store the data received from a passive connection or from a PORT/EPRT connection to a file. If the filename exists, it will append the data.
STOR /path/something.txt Like APPE but it will overwrite the files
STOU /path/something.txt Like APPE, but if exists it wont do anything.
RETR /path/to/file A passive or a port connection must be establish. Then, the FTP server will send the indicated file through that connection
REST 6 This will indicate the server that next time it send something using RETR it should start in the 6th byte.
TYPE i Set transfer to binary
PASV This will open a passive connection and will indicate the user were he can connects 
```

### Filezilla Server Vulnerability
FileZilla usually binds to local an Administrative service for the FileZilla-Server (port 14147). If you can create a tunnel from your machine to access this port, you can connect to it using a blank password and create a new user for the FTP service.
### Config files
```
ftpusers
ftp.conf
proftpd.conf
```
### HackTricks Automatic Commands
```
Protocol_Name: FTP    #Protocol Abbreviation if there is one.
Port_Number:  21     #Comma separated if there is more than one.
Protocol_Description: File Transfer Protocol          #Protocol Abbreviation Spelled out

Entry_1:
  Name: Notes
  Description: Notes for FTP
  Note: |
    Anonymous Login
    -bi     <<< so that your put is done via binary

    wget --mirror 'ftp://ftp_user:UTDRSCH53c"$6hys@10.10.10.59'
    ^^to download all dirs and files

    wget --no-passive-ftp --mirror 'ftp://anonymous:anonymous@10.10.10.98' 
    if PASV transfer is disabled

    https://book.hacktricks.xyz/pentesting/pentesting-ftp

Entry_2:
  Name: Banner Grab
  Description: Grab FTP Banner via telnet
  Command: telnet -n {IP} 21

Entry_3:
  Name: Cert Grab
  Description: Grab FTP Certificate if existing
  Command: openssl s_client -connect {IP}:21 -starttls ftp

Entry_4:
  Name: nmap ftp
  Description: Anon login and bounce FTP checks are performed
  Command: nmap --script ftp-* -p 21 {IP}

Entry_5:
  Name: Browser Connection
  Description: Connect with Browser
  Note: ftp://anonymous:anonymous@{IP}

Entry_6:
  Name: Hydra Brute Force
  Description: Need Username
  Command: hydra -t 1 -l {Username} -P {Big_Passwordlist} -vV {IP} ftp
```