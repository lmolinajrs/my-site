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

### <span style="color:#b31d36">Full Interactive

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

### <span style="color:#b31d36">Python
```python
python -c 'import pty; pty.spawn("/bin/bash")'
```
### <span style="color:#b31d36">Echo
```python
echo 'os.system('/bin/bash')'
```
### <span style="color:#b31d36">Sh
```bash
/bin/sh -i
```
### <span style="color:#b31d36">Bash
```bash
/bin/bash -i
```
### <span style="color:#b31d36">Perl
```perl
perl -e 'exec "/bin/bash";'
```
### <span style="color:#b31d36">Ruby
```ruby
exec "/bin/bash"
```
### <span style="color:#b31d36">Lua
```lua
os.execute('/bin/bash')
```
### <span style="color:#b31d36">From Within IRB
```shell
exec "/bin/bash"
```
### <span style="color:#b31d36">From Within Vi
```
:!bash
:set shell=/bin/bash:shell
```
### <span style="color:#b31d36">From Within Nmap
```
!sh
```