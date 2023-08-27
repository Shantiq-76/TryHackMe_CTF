# VulnNet: Internal

We have to get four flags.
- services.txt
- internal flag
- user.txt
- root.txt

## nmap scan
PORT      STATE    SERVICE     VERSION \
22/tcp    open     ssh         OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0) \
111/tcp   open     rpcbind     2-4 (RPC #100000) \
139/tcp   open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP) \
445/tcp   open     netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP) \
873/tcp   open     rsync       (protocol version 31) \
2049/tcp  open     nfs_acl     3 (RPC #100227) \
6379/tcp  open     redis       Redis key-value store \
9090/tcp  filtered zeus-admin \
38425/tcp open     mountd      1-3 (RPC #100005) \
42341/tcp open     mountd      1-3 (RPC #100005) \
45895/tcp open     nlockmgr    1-4 (RPC #100021) \
46479/tcp open     mountd      1-3 (RPC #100005) \
Service Info: Host: VULNNET-INTERNAL; OS: Linux; CPE: cpe:/o:linux:linux_kernel \

We have much of open ports..
- ssh
- rpcbind \
  --> handles the assignment of RPC requests (Remote Procedure Calls) to network services
- 2x netbios-ssn \
  --> samba
- rsync \
  --> synchonisation of data
- nfs-acl \
  --> Control of access rights
- redis \
  --> noSQL DB
- zeus-admin \
  --> http
- 2x mountd \
  --> Remote procedure call (RPC) that answers a client request to mount a file system.
- nlockmgr \
  --> NFS

Hcg3HP67@TW@Bc72v
"B65Hx562F@ggAZ@F"
10.10.168.176
