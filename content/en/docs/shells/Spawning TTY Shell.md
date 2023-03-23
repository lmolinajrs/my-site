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

### <span style="color:#208355">Full Interactive

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

### <span style="color:#208355">Python
```python
python -c 'import pty; pty.spawn("/bin/bash")'
```
### <span style="color:#208355">Echo
```python
echo 'os.system('/bin/bash')'
```
### <span style="color:#208355">Sh
```bash
/bin/sh -i
```
### <span style="color:#208355">Bash
```bash
/bin/bash -i
```
### <span style="color:#208355">Perl
```perl
perl -e 'exec "/bin/bash";'
```
### <span style="color:#208355">Ruby
```ruby
exec "/bin/bash"
```
### <span style="color:#208355">Lua
```lua
os.execute('/bin/bash')
```
### <span style="color:#208355">From Within IRB
```shell
exec "/bin/bash"
```
### <span style="color:#208355">From Within Vi
```
:!bash
:set shell=/bin/bash:shell
```
### <span style="color:#208355">From Within Nmap
```
!sh
```