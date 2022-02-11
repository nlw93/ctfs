# Brainstorm

## Recon
### Scan

This room is a bit challenging. The scan shows ftp (21) and some chat client (9999). I only see these two, but apparently there are 6. Must be some UDP ports I missed.

### Explore the services

#### FTP
Anonymous login is allowed. I logged in with user `ftp` and pass `ftp` but kept hitting an error about **Extended Passive Mode.**. I did some reasearch to refresh my memory on what this was.
```bash
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
ftp 10.10.109.7 -A                                                      
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

I was able to get everything working in Windows 11. Had to configure Windows firewall to allow port 9999 though.

Everything is working!

![[Pasted image 20220211005230.png]]

Seems like the overflow will be in the username. Lets fire up the tool.

Looks like I spoke too soon about everything working. I got an error when trying to run a `!mona` command.

> pycommands: error importing module

I see online this error is from using the 64-bit python and using a 32bit version should fix it. So...reinstall python with x86 version.

Also, I need to install [mona](https://github.com/corelan/mona). Wow, setting up this environment is a PITA - it was nice having a box ready on TryHackMe for the other challenges. I'll have to save this setup.

Another issue - since the file was downloaded from the internet, it has to be marked as safe in the file properties before it can be used.

![[Pasted image 20220211015656.png]]

Another issue I ran into was the `mona.py` file was full of HTML as I downloaded it incorrectly. Found that [here on github](https://github.com/corelan/mona/issues/15)


IT WORKS!!!!

Getting a snapshot...

I used the buffer overflow helper script I made and got this box done MUCH quicker than I was able to get the DEV environment set up. 
![[Pasted image 20220211021145.png]]

I never turned off Windows AV module so I only had access for a bit before getting kicked.

![[Pasted image 20220211021322.png]]

The completed exploit is as follows:

```python3


