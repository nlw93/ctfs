# Enumeration
## Port scan
```
PORT     STATE SERVICE      REASON          VERSION
80/tcp   open  http         syn-ack ttl 127 Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
| http-title: Secure Notes - Login
|_Requested resource was login.php
|_http-server-header: Microsoft-IIS/10.0
445/tcp  open  microsoft-ds syn-ack ttl 127 Windows 10 Enterprise 17134 microsoft-ds (workgroup: HTB)
8808/tcp open  http         syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-title: IIS Windows
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows XP|98 (88%)
```
## Port 80
### nmap script scan
![[ca89d84f057540d393bc61cd9fcddc29.png]]
### whatweb
`Summary: RedirectLocation[login.php],`
`Microsoft-IIS[10.0], `
`PHP[7.2.7], `
`HTTPServer[Microsoft-IIS/10.0], `
`Cookies[PHPSESSID], `
`X-Powered-By[PHP/7.2.7]`
### dirbuster
```
 cat tcp_80_http_feroxbuster_* | grep 200           
200       35l       85w     1223c http://10.10.10.97/Login.php
200       35l       85w     1223c http://10.10.10.97/login.php
200       41l      106w     1569c http://10.10.10.97/register.php
200       35l       85w     1223c http://10.10.10.97/Login.php
200       35l       85w     1223c http://10.10.10.97/login.php
200       41l      106w     1569c http://10.10.10.97/register.php
```
Cookie
![[444632026ec147438904abff8effe875.png]]
## Tells you if a user exists
![[fa3fca7963374518a13fc57ba70b6079.png]]
## Create acct to view the inside.
### Login success


## Port 445
```
smb-enum-shares: 
|   note: ERROR: Enumerating shares failed, guessing at common ones (NT_STATUS_ACCESS_DENIED)
|   account_used: <blank>
|   \\10.10.10.97\ADMIN$: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|     Anonymous access: <none>
|   \\10.10.10.97\C$: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|     Anonymous access: <none>
|   \\10.10.10.97\IPC$: 
|     warning: Couldn't get details for share: NT_STATUS_ACCESS_DENIED
|_    Anonymous access: READ
```

 

## Port 8808
```
200       32l       54w      696c http://10.10.10.97:8808/
```
![[bc4b99b7800a4b3ea5be7ea9473516b9.png]]
# Weaponization
## PHP 7.2.7
![[2c0cef4cfabe4d7992d54c043bfa68aa.png]]

## IIS 10.0


## Windows Server

# This is where I gave up.
## Spoiler
Initial foothold was to exploit SQLi on the register page to create an account with `' OR 1 OR '`
![[f49b16ef3cf949bba30c2c3a4e39c7a8.png]]

---
# SMB access
**destination**: `\\secnotes.htb\new-site`
**user**: `tyler`
**pass**: `92g!mA8BGjOirkL%OG*&`

---

Placing reverse php shell on the **webserver** running on **port 8808**.

![[7cea7609c1e94c02b7b5afff4ad88f52.png]]

Netcat listening on 8001
Load the exploit by going to http://10.10.10.97:8808/php-reverse-shell.php

### Php reverse shell is not working, there is something removing the file often.

Load this code instead
![[eeb7390220684674823dcc527a59c7f6.png]]
Along with the **nc.exe**
## We got a shell
![[e709eeb63b5949c3b57f14c9866dd980.png]]
## User flag
![[c794096cdbd6426497a5edcc370e3675.png]]
`c89cf1cc7cd0acfd5c4adf273ca75a52`

## Find WLS
![[3710d4d46b804bf88b253465aa92a826.png]]
`where /R C:\windows bash.exe`

Use the same method as above to load the remote service
![[abd3c6767c8c463e98f7a2cb1c012460.png]]
![[27a4f0648c274b7ab590ed2a409c4c56.png]]

## Spawn a tty session
![[d6e419e9ceca4b8f8ab59bc6f38ea1b3.png]]

Found something interesting in history.
![[f7884df62ed94db395609ac2835f6eb7.png]]

Found **root.txt**
![[7ba7ce10d1d64d6598734aecacdf4e67.png]]
![[dedaf29183dc48e680633829431a36fe.png]]
**root flag**: `9654cd0c73aa2e0db9b99a11679018c4`