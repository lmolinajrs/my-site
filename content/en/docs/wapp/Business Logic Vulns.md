---
title: "Business Logic Vulns"
draft: false
images: []
description: "Business Logic Vulnerabilities"
lead: "Business Logic Vulnerabilities"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## Parameter Manipulation
### Price
Maybe you can change a numeric data type to a negative number
### Quantity
Change item quantity to a negative value or a decimal one.
You can also send empty request from burpsuite until application crashes. Example: Wee add an exessive amount of items (like 3.000.000) and after that we will see a negative value.
### Currency
If it is in USD/Eur try to change in INR
### Email
Send an extra large email. You can use metasploit to generate it.
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 255
```