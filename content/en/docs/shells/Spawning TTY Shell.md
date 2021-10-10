---
title: "Spawning TTY Shell"
draft: false
images: []
description: "Full interactives shells, spawning shells, etc"
lead: "Full interactives shells, spawning shells, etc"
menu:
  docs:
    parent: "Shells"
weight: 1
toc: true
---

### Full Interactive

```
script /dev/null -c bash
Ctrl+Z
stty raw -echo
fg
reset
xterm
export TERM=xterm
export shell=bash
########################
stty -a
stty rows XX columns XX
```

#### Python
```
python -c 'import pty; pty.spawn("/bin/bash")'
```
### Echo
```
echo 'os.system('/bin/bash')'
```
### Sh
```
/bin/sh -i
```
### Bash
```
/bin/bash -i
```
#### Perl
```
perl -e 'exec "/bin/bash";'
```
### Ruby
```
exec "/bin/bash"
```
### Lua
```
os.execute('/bin/bash')
```
### From Within IRB
```
exec "/bin/bash"
```
### From Within Vi
```
:!bash
:set shell=/bin/bash:shell
```
### From Within Nmap
```
!sh
```