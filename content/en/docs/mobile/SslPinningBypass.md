---
title: "SSL Pinning Bypass"
Description: Bypassing SSL Pinning
draft: false
images: []
menu:
  docs:
    parent: "Mobile"
weight: 4
toc: true
content: SSL Pinning Bypass
---
### <span style="color:#208355">First Option

Look for the app binary identifier, something like "com.testing.pentestingmobile"

```
frida-ps -Uai
```

Download and save <a href="https://raw.githubusercontent.com/sha-16/Frida-Scripts/main/bypass_sslpinning.js" target="_blank">this script</a></span> as bypass_sslpinning.js

```
frida -U -f <AppIdentifier> -l bypass_sslpinning.js
```

### <span style="color:#208355">Second Option

Install objection with pip
```
pip3 install objection
```

Look for the app binary identifier, something like "com.testing.pentestingmobile"

```
frida-ps -Uai
```



Run objection
```
objection --gadget <AppIdentifier> explore
```
Disable SSL Pinning
```
Android sslpinning disable
```