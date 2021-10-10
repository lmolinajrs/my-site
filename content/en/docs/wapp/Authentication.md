---
title: "Authentication"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## User Enumeration
### Username enumeration via error messages
* Username and password doesn't match
* Username does not exist
* Etc
### Username enumeration via account lock
Select the attack type "Cluster bomb". Add a payload position to the username parameter. Add a blank payload position to the end of the request

username=§invalid-username§&password=example§§

Notice that the responses for one of the usernames were longer than responses when using other usernames. Study the response more closely and notice that it contains a different error message like: You have made too many incorrect login attempts.
## Evading authentication brute force filters

### X-Forwarded-For
Identify that the X-Forwarded-For header is supported, which allows you to spoof your IP address and bypass the IP-based brute-force protection.
### Password brute force protection
Notice that you can reset the counter for the number of failed login attempts by logging in to your own account before this limit is reached
```
User: Invalid     -    Password: Any
User: Invalid     -    Password: Any
User: Valid        -    Password: Correct
User: Invalid     -    Password: Any
User: Invalid     -    Password: Any
User: Valid        -    Password: Correct
```
### Multiple credentials per request
In Burp Repeater, replace the single string value of the password with an array of strings containing all of the candidate passwords. For example, in JSON format:
```
"username" : "carlos",
"password" : [
    "123456",
    "password",
    "qwerty"
    ...
]
```
## 2FA Bypass
### Simple Bypass
* Login with your account
* Go to your account page and copy the url
* Log out. Login with victim credentials
* Go to the the url copied before
### Direct Bypass
Try accessing to the next endpoint directly. If this doesn't work, try to change the Referrer header as if you came from the 2FA page.
### Sharing unused tokens
Check if you can get for your account a token and try to use it to bypass the 2FA in a different account.