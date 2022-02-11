# Brainstorm

## Recon
### Scan

This room is a bit challenging. The scan shows ftp (21) and some chat client (9999). I only see these two, but apparently there are 6. Must be some UDP ports I missed.

### Explore the services

#### FTP
Anonymous login is allowed. I logged in with user `ftp` and pass `ftp` but kept hitting an error about **Extended Passive Mode.**. I did some reasearch to refresh my memory on what this was.
```
ftp 10.10.109.7
Connected to 10.10.109.7.
220 Microsoft FTP Service
Name (10.10.109.7:nate): ftp
331 Anonymous access allowed, send identity (e-mail name) as password.
Password:
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
229 Entering Extended Passive Mode (|||49356|) 
```

Long story short, you need to force active mode since passive mode is failing. I spent a lot of time googling this error and reading about how to fix it. But the answer was right in the damn man page; need to force active mode with the `-A` flag.

```bash
$> ftp 10.10.109.7 -A                                                      
Connected to 10.10.109.7.                                                                                      
220 Microsoft FTP Service
Name (10.10.109.7:nate): ftp                                                                                   
331 Anonymous access allowed, send identity (e-mail name) as password.                                         
Password:                                                                                                      
230 User logged in.                                                                                            
Remote system type is Windows_NT.                                                                              
ftp> ls                                                                                                        
200 EPRT command successful.
150 Opening ASCII mode data connection.
08-29-19  07:36PM       <DIR>          chatserver
226 Transfer complete.
```

Great, it's working now and I see a folder.

```bash
ftp> cd chatserver
250 CWD command successful. 
ftp> ls
200 EPRT command successful.
150 Opening ASCII mode data connection.
08-29-19  09:26PM                43747 chatserver.exe
08-29-19  09:27PM                30761 essfunc.dll
226 Transfer complete.
```

Find `chatserver.exe` and download it for later.

```bash
ftp> get chatserver.exe
local: chatserver.exe remote: chatserver.exe
200 EPRT command successful.
150 Opening ASCII mode data connection.
100% |******************************************************************| 43747       43.33 KiB/s    00:00 ETA
226 Transfer complete.
WARNING! 45 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
43747 bytes received in 00:00 (43.31 KiB/s)
```

Crap, I've ran into this before. Need to use `binary` to transfer executables.

```bash
ftp> binary
200 Type set to I.
ftp> get chatserver.exe
local: chatserver.exe remote: chatserver.exe
200 EPRT command successful.
125 Data connection already open; Transfer starting.
100% |******************************************************************| 43747       43.87 KiB/s    00:00 ETA
226 Transfer complete.
```







It appears to be working. Will not have to to do anything crafty with the network.

![[Pasted image 20220210233944.png]]

Then install immunity, which also installs python. The python installer failed the first time around so I selected "Install just for this user" the second time around. Not sure if it was coincidence or lucky guess but it worked. Lets get crackin!

Actually immunity wouldn't boot as it coulnd't find the python files. So I'm moving this project to a dev version of Windows 11 I have installed in the lab. It'll be a good first look at Windows 11 anyways.





