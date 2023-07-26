---
title: "Root Bypass"
Description: Bypassing root limitation on android devices
draft: false
images: []
menu:
  docs:
    parent: "Mobile"
weight: 3
toc: true
content: Anti Root
---

### <span style="color:#208355">First Option

Look for the app binary identifier, something like "com.testing.pentestingmobile"

```
frida-ps -Uai
```

Download and save <a href="https://codeshare.frida.re/@dzonerzy/fridantiroot/" target="_blank">this script</a></span> as antiroot.js

Close the application in the emulator and then just run the following command.

```
frida -U -f <AppIdentifier> -l antiroot.js
```

### <span style="color:#208355">Second Option

Look for the app binary identifier, something like "com.testing.pentestingmobile"

```
frida-ps -Uai
```

Download and save <a href="https://codeshare.frida.re/@KishorBal/multiple-root-detection-bypass/" target="_blank">this script</a></span> as multiple-root-detection-bypass.js

Close the application in the emulator and then just run the following command.

```
frida -U -f <AppIdentifier> -l multiple-root-detection-bypass.js
```
---
