---
title: "Parameter Pollution"
draft: false
images: []
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

For example, there is a URL https://www.anybank.com/send which has three parameters :
* from :
* to :
* amount :

**URL : https://www.anybank.com/send/?from=accountA&to=accountB&amount=10000**

Now this is a normal URL which will proceed a transaction of 10000 from accountA to accountB but what if we add another same parameter “from :”

So the URL will be like **https://www.anybank.com/send/?from=accountA&to=accountB&amount=10000&from=accountC**