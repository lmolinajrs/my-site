---
title: "Linux"
draft: false
images: []
menu:
  docs:
    parent: "Privesc"
weight: 100
toc: true
---

### Common Linux privilege escalation techniques
* Kernel exploits
* Programs running as root
* Installed software
* Weak/reused/plaintext passwords
* Inside service
* Suid misconfiguration
* Abusing sudo-rights
* World writable scripts invoked by root
* Bad path configuration
* Cronjobs
* Unmounted filesystems

## Privilege Escalation Techniques
### Kernel Exploits
```
uname -a
cat /proc/version
cat /etc/issue
```
#### Search for exploits
```
site:exploit-db.com kernel version
searchsploit kernel version
python linprivchecker.py 
```
### Programs running as root
```
ps aux | grep -i root
```
### User Installed Software
```
# Common locations for user installed software
/usr/local/
/usr/local/src
/usr/local/bin
/opt/
/home
/var/
/usr/src/
```
### Weak/reused/plaintext passwords
Check file where webserver connect to database (config.php or similar)
Check databases for admin passwords that might be reused
Check weak passwords
```
grep -ir 'password'
./LinEnum.sh -t -k password
```
### Service only available from inside
```
netstat -anlp
netstat -ano
```
### Suid and Guid Misconfiguration
```
#Find SUID
find / -perm -u=s -type f 2>/dev/null

#Find GUID
find / -perm -g=s -type f 2>/dev/null
```
If you found something interesting then you can take a look at GTFOBins
### World writable scripts invoked as root
If you find a script that is owned by root but is writable by anyone you can add your own malicious code in that script that will escalate your privileges when the script is run as root.
```
#World writable files directories
find / -writable -type d 2>/dev/null
find / -perm -222 -type d 2>/dev/null
find / -perm -o w -type d 2>/dev/null

 World executable folder
find / -perm -o x -type d 2>/dev/null

 World writable and executable folders
find / \( -perm -o w -perm -o x \) -type d 2>/dev/null
```
### Cronjob
```
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
```