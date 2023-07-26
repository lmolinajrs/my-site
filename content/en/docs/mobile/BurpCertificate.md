---
title: "Configuring Burpsuite"
Description: How to import and configure burpsuite with android emulator
draft: false
images: []
menu:
  docs:
    parent: "Mobile"
weight: 2
toc: true
content: Installing Burpsuite Certificate
---

First, we must have installed <span style="color:#208355"><a href="https://www.genymotion.com/" target="_blank">Genymotion</a></span>, adb, and have an emulated mobile device.

### <span style="color:#208355">Install the Certificate

Download CA certificate and install it in Configuration > CA Certificates > Install/Import

### <span style="color:#208355">Configure the Proxy

Use the following adb commands to set the proxy in the android device

```
.\adb.exe shell settings put global http_proxy localhost:3333
```
```
.\adb.exe shell "iptables -t nat -F"
```
```
.\adb.exe shell "iptables -t nat -A OUTPUT -p tcp --dport 443 -j DNAT --to-destination 127.0.0.1:3333
```
```
.\adb.exe shell "iptables -t nat -A POSTROUTING -p tcp --dport 443 -j MASQUERADE"
```
```
.\adb.exe reverse tcp:3333 tcp:8080
```