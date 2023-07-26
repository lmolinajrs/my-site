---
title: "Installing Frida"
Description: How to install and configure frida server
draft: false
images: []
menu:
  docs:
    parent: "Mobile"
weight: 1
toc: true
content: intallingfrida
---

### <span style="color:#208355">Download Frida

First, we must have installed <span style="color:#208355"><a href="https://www.genymotion.com/" target="_blank">Genymotion</a></span>, adb, and have an emulated mobile device.

Next, we need to execute the following command to identify the version of Frida Server that should be downloaded. <span style="color:#208355"><a href="https://github.com/frida/frida" target="_blank">Downloand Here </a>

```
.\adb.exe shell getprop ro.product.cpu.abi
```
### <span style="color:#208355">Running Frida

```
.\adb.exe devices
```

```
.\adb.exe push frida-server-15.2.2-android-x86 "/data/local/tmp".
```

```
.\adb.exe shell "chmod 755 /data/local/tmp/frida-server-15.2.2-android-x86".
```

```
.\adb.exe shell "/data/local/tmp/frida-server-15.2.2-android-x86"
```