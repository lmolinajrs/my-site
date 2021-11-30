---
title: "Fuzzing Oneliners"
draft: false
images: []
description: "Some oneliners for recon and fuzzing"
lead: "Some oneliners for recon and fuzzing"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

### Extract Endpoints from JS File
```
cat main.js | grep -oh "\"\/[a-zA-Z0-9_/?=&]*\"" | sed -e 's/^"//' -e 's/"$//' | sort -u
```
### Find JS Files
```
assetfinder site.com | gau|egrep -v '(.css|.png|.jpeg|.jpg|.svg|.gif|.wolf)'|while read url; do vars=$(
```
### Get all the urls out of a sitemap.xml
```
curl -s domain.com/sitemap.xml | xmllint --format - | grep -e 'loc' | sed -r 's|</?loc>||g'
```
### Subdomain Bruteforcer with FFUF
```
ffuf -u https://FUZZ.rootdomain -w jhaddixall.txt -v | grep "| URL |" | awk '{print $4}'
```
### Export ffuf as .json then use this
```
cat result.txt | jq -r '.results[] | [.url,.redirectlocation,.status,.length] | "\(.[0]) -> \(.[1]) \(.[2]),\(.[3])"'
```
### Waybackurls to burpsuite sitemap
```
echo 'https://www.test.com/' | waybackurls | parallel -j 4 -q curl -sk -o /dev/null --proxy http://127.0.0.1:8080/
```
### GoSpider output to burpsuite sitemap
```
gospider -s https://site.com -d 16 -a -w -r -a -c 50 | grep -oP '(http|https)://[^/"].*' | cut -d "]" -f1 | parallel -j 10 -q curl -sk -o /dev/null --proxy http://127.0.0.1:8080
```