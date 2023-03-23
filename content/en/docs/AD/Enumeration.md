---
title: "Initial Enumeration"
Description: External recon, enumeration principles, initial domain enumeration
draft: false
images: []
menu:
  docs:
    parent: "Active Directory"
weight: 1
toc: true
---

### <span style="color:#208355"> Finding Address Spaces

The <span style="color:#208355"><a href="https://bgp.he.net/" target="_blank">BGP-Toolkit</a></span> hosted by Hurricane Electric is a fantastic resource for researching what address blocks are assigned to an organization and what ASN they reside within. Just punch in a domain or IP address, and the toolkit will search for any results it can. 

### <span style="color:#208355"> DNS

DNS is a great way to validate our scope and find out about reachable hosts the customer did not disclose in their scoping document. Sites like <span style="color:#208355"><a href="https://whois.domaintools.com/" target="_blank">domaintools</a></span>, and <span style="color:#208355"><a href="https://viewdns.info/" target="_blank">viewdns.info</a></span> are great spots to start. We can get back many records and other data ranging from DNS resolution to testing for DNSSEC and if the site is accessible in more restricted countries.

```
nslookup ns1.inlanefreight.com
```

### <span style="color:#208355"> Hunting For Files

```
filetype:pdf inurl:example.com
```

### <span style="color:#208355"> Hunting E-mail Addresses

```python
intext:"@example.com" inurl:example.com
```

### <span style="color:#208355"> Username Harvesting

We can use a tool such as <span style="color:#208355"><a href="https://github.com/initstring/linkedin2username" target="_blank">linkedin2username</a></span> to scrape data from a company's LinkedIn page and create various mashups of usernames (flast, first.last, f.last, etc.) that can be added to our list of potential password spraying targets.

### <span style="color:#208355"> Credential Hunting

<span style="color:#208355"><a href="https://dehashed.com" target="_blank">Dehashed</a></span> or <span style="color:#208355"><a href="https://leakpeek.com" target="_blank">LeakPeek</a></span>

## <span style="color:#208355"> Identifying Hosts

### <span style="color:#208355"> Wireshark

We can use Wireshark and TCPDump to "put our ear to the wire" and see what hosts and types of network traffic we can capture. This is particularly helpful if the assessment approach is "black box". Notice some ARP requests and replies, MDNS or layer two packets.

```
sudo tcpdump -i ens224 
```
### <span style="color:#208355"> Nbtscan Active Checks
NBTscan is a program for scanning IP networks for NetBIOS name information. It sends NetBIOS status query to each address in supplied range and lists received information in human readable form.
```python
for i in {0..254}; do nbtscan 192.168.$i.0/24; done
```

### <span style="color:#208355"> FPing Active Checks

Here we'll start fping with a few flags: 

* a: to show targets that are alive,
* s: to print stats at the end of the scan,
* g: to generate a target list from the CIDR network
* q: to not show per-target results.

```
fping -asgq 172.16.5.0/23
```

### <span style="color:#208355"> Nmap Waf Bypass

```
sudo nmap -Pn -sS --script firewall-bypass --source-port 53 -v -iL hosts.txt -p- --open --min-rate=2000 -oN nmapFullTCP.txt
```
### <span style="color:#208355"> Finding the Domain Controller

```
nslookup -type=SRV _ldap._tcp.example.local
```

### <span style="color:#208355"> Kerbrute - Username Enumeration

```
kerbrute userenum -d EXAMPLE.LOCAL --dc 172.16.5.5 usernames.txt -o valid_ad_users
```