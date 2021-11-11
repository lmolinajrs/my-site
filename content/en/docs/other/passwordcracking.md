---
title: "Password Cracking"
Description: Cracking passwords with haschat and john
draft: false
images: []
menu:
  docs:
    parent: "Other"
weight: 1
toc: true
---

## John The Ripper
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash
john --rules --wordlist=/usr/share/wordlists/rockyou.txt hash
```

## Hashcat
```
# Dictionary
hashcat -m 0 -a 0 hashfile dictionary.txt -O --user -o result.txt

# Dictionary + rules
hashcat -m 0 -w 3 -a 0 hashfile dictionary.txt -O -r haku34K.rule --user -o result.txt

# Mask bruteforce (length 1-8 A-Z a-z 0-9)
hashcat -m 0 -w 3 -a 3 hashfile ?1?1?1?1?1?1?1?1 --increment -1 --user ?l?d?u
hashcat -m 0 -w 3 -a 3 hashfile suffix?1?1?1 -i -1 --user ?l?d

# Modes
-a 0 = Dictionary (also with rules)
-a 3 = Bruteforce with mask 

# Max performance options
--force -O -w 3 --opencl-device-types 1,2

# Output results
-o result.txt

# Ignore usernames in hashfile
--user/--username

# Masks
?l = abcdefghijklmnopqrstuvwxyz
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
?d = 0123456789
?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
?a = ?l?u?d?s
?b = 0x00 - 0xff
```

## Useful hashes
### Linux Hashes - /etc/shadow

|      ID    |   DESCRIPTION   |
|----------|:-------------:|
| 500   | md5crypt $1$, MD5(Unix)  |
| 200   | bcrypt $2*$, Blowfish(Unix) | 
| 400   | sha256crypt $5$, SHA256(Unix) |
| 1800  | sha512crypt $6$, SHA512(Unix) |

### Windows Hashes

|      ID    |   DESCRIPTION  |
|----------|:-------------:|
| 3000   | LM  |
| 1000   | NTLM | 

### Common Hashes

|      ID    |   DESCRIPTION  |
|----------|:-------------:|
| 900   | MD4  |
| 0   | MD5 | 
| 5100 | Half MD5|
| 100 | SHA1|
| 10800 | SHA-384|
| 1400 | SHA-256|
| 1700 | SHA-512| 