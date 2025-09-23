# Golang for security
This is a repo for documenting my learning process of security practices and applying my knowledges in Go.

## Footprinting
An example of footprinting is searching for data from someone filtering by website:
```
camilo de lellis site:ifrn.edu.br
```

I found my institution ID in this [website](https://portal.ifrn.edu.br/documents/4997/Selecionados_Alimenta%C3%A7%C3%A3o.pdf). Here it is:
```
20231038060019
```

Say that I want to find a dissertation linked to my name, let's say not exactly mine, but [this file](https://saocamilo-sp.br/assets/artigo/bioethikos/78/Art12.pdf). For searching information of a specific filetype, we could use the following search term:
```
camilo de lellis dissertação filetype:pdf
```

## Port scan
Copying compose.yml:
```sh
wget https://www.segurancaderedes.com.br/spd/pratica02/docker-compose.yml
```

Accessing the container:
```sh
docker exec -it scan01 bash
```

Verifying the connection to the target machine:
```sh
ping 172.30.0.101
PING 172.30.0.101 (172.30.0.101) 56(84) bytes of data.
64 bytes from 172.30.0.101: icmp_seq=1 ttl=64 time=0.162 ms
64 bytes from 172.30.0.101: icmp_seq=2 ttl=64 time=0.081 ms
64 bytes from 172.30.0.101: icmp_seq=3 ttl=64 time=0.086 ms
64 bytes from 172.30.0.101: icmp_seq=4 ttl=64 time=0.091 ms
^C
--- 172.30.0.101 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3074ms
rtt min/avg/max/mdev = 0.081/0.105/0.162/0.033 ms
```

Running the nmap command to port scan it:
```sh
nmap 172.30.0.101
Starting Nmap 7.93 ( https://nmap.org ) at 2025-09-23 17:04 UTC
Nmap scan report for alvo01.scan01_my-network (172.30.0.101)
Host is up (0.0000050s latency).
Not shown: 981 closed tcp ports (reset)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
23/tcp   open  telnet
25/tcp   open  smtp
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
512/tcp  open  exec
513/tcp  open  login
514/tcp  open  shell
1099/tcp open  rmiregistry
1524/tcp open  ingreslock
2121/tcp open  ccproxy-ftp
3306/tcp open  mysql
5432/tcp open  postgresql
6667/tcp open  irc
8009/tcp open  ajp13
8180/tcp open  unknown
MAC Address: FA:32:4C:88:39:15 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.19 seconds
```

Running nmap to scan all TCP ports of the target:
```sh
nmap -p- 172.30.0.101
Starting Nmap 7.93 ( https://nmap.org ) at 2025-09-23 17:08 UTC
Nmap scan report for alvo01.scan01_my-network (172.30.0.101)
Host is up (0.0000060s latency).
Not shown: 65512 closed tcp ports (reset)
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
23/tcp    open  telnet
25/tcp    open  smtp
80/tcp    open  http
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
512/tcp   open  exec
513/tcp   open  login
514/tcp   open  shell
1099/tcp  open  rmiregistry
1524/tcp  open  ingreslock
2121/tcp  open  ccproxy-ftp
3306/tcp  open  mysql
3632/tcp  open  distccd
5432/tcp  open  postgresql
6667/tcp  open  irc
6697/tcp  open  ircs-u
8009/tcp  open  ajp13
8180/tcp  open  unknown
8787/tcp  open  msgsrvr
35157/tcp open  unknown
MAC Address: FA:32:4C:88:39:15 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.82 seconds
```

To put the result into a file:
```sh
nmap -p- 172.30.0.101 > resultado.scan
```

Run nmap individually to confirm each service and their versions. Below, running on port 21:
```sh
nmap -sT -p 21 -sV 172.30.0.101
Starting Nmap 7.93 ( https://nmap.org ) at 2025-09-23 17:26 UTC
Nmap scan report for alvo01.scan01_my-network (172.30.0.101)
Host is up (0.000058s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
MAC Address: FA:32:4C:88:39:15 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 0.39 seconds
```

Use nmap's module 'scripts':
```sh
nmap -sT -p 21 -sV 172.30.0.101 -script=vuln
Starting Nmap 7.93 ( https://nmap.org ) at 2025-09-23 17:29 UTC
Nmap scan report for alvo01.scan01_my-network (172.30.0.101)
Host is up (0.000032s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.3.4
| ftp-vsftpd-backdoor: 
|   VULNERABLE:
|   vsFTPd version 2.3.4 backdoor
|     State: VULNERABLE (Exploitable)
|     IDs:  CVE:CVE-2011-2523  BID:48539
|       vsFTPd version 2.3.4 backdoor, this was reported on 2011-07-04.
|     Disclosure date: 2011-07-03
|     Exploit results:
|       Shell command: id
|       Results: uid=0(root) gid=0(root)
|     References:
|       https://github.com/rapid7/metasploit-framework/blob/master/modules/exploits/unix/ftp/vsftpd_234_backdoor.rb
|       https://www.securityfocus.com/bid/48539
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2011-2523
|_      http://scarybeastsecurity.blogspot.com/2011/07/alert-vsftpd-download-backdoored.html
MAC Address: FA:32:4C:88:39:15 (Unknown)
Service Info: OS: Unix

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 11.66 seconds
```

<!--
## Google Hacking(?)

## Steganography

## Cryptography
-->