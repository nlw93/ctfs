# Gatekeeper

## Recon

There not much for an introduction on this one. The description mentions passing through the gatekeeper and there's fire on the other side.

### Scanning
#### NMAP
```bash
PORT      STATE SERVICE      REASON          VERSION
135/tcp   open  msrpc        syn-ack ttl 125 Microsoft Windows RPC
139/tcp   open  netbios-ssn  syn-ack ttl 125 Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds syn-ack ttl 125 Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
31337/tcp open  Elite?       syn-ack ttl 125
| fingerprint-strings: 
|   GetRequest: 
|     Hello GET / HTTP/1.0
|_    Hello
49152/tcp open  unknown      syn-ack ttl 125
49153/tcp open  unknown      syn-ack ttl 125
49154/tcp open  unknown      syn-ack ttl 125
49155/tcp open  unknown      syn-ack ttl 125
49161/tcp open  unknown      syn-ack ttl 125
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port31337-TCP:V=7.92%I=9%D=2/11%Time=62072C3C%P=x86_64-pc-linux-gnu%r(G
SF:etRequest,24,"Hello\x20GET\x20/\x20HTTP/1\.0\r!!!\nHello\x20\r!!!\n");
OS fingerprint not ideal because: Didn't receive UDP response. Please try again with -sSU
Aggressive OS guesses: Microsoft Windows 7 or Windows Server 2008 R2 (95%), Microsoft Windows 7 SP1 (95%), Microsoft Windows Home Server 2011 (Windows Server 2008 R2) (94%)
```

#### SMBmap
```bash
[+] Guest session       IP: 10.10.79.126:139    Name: 10.10.79.126
        Disk                                                    Permissions      Comment
        ----                                                    -----------      -------
        ADMIN$                                                  NO ACCESSRemote Admin
        C$                                                      NO ACCESSDefault share
        IPC$                                                    NO ACCESSRemote IPC
        Users                                                   READ ONLY
        .\Users\*
        dw--w--w--                0 Thu May 14 20:57:08 2020    .
        dw--w--w--                0 Thu May 14 20:57:08 2020    ..
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Default
        fr--r--r--              174 Tue Apr 21 22:18:13 2020    desktop.ini
        dr--r--r--                0 Thu May 14 20:58:07 2020    Share
        .\Users\Default\*
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    .
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    AppData
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Desktop
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Documents
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Downloads
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Favorites
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Links
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Music
        fr--r--r--           262144 Tue Apr 21 22:18:13 2020    NTUSER.DAT
        fr--r--r--             1024 Tue Apr 21 22:18:13 2020    NTUSER.DAT.LOG
        fr--r--r--           189440 Tue Apr 21 22:18:13 2020    NTUSER.DAT.LOG1
        fr--r--r--                0 Tue Apr 21 22:18:13 2020    NTUSER.DAT.LOG2
        fr--r--r--            65536 Tue Apr 21 22:18:13 2020    NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TM.blf
        fr--r--r--           524288 Tue Apr 21 22:18:13 2020    NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000001.regtrans-ms
        fr--r--r--           524288 Tue Apr 21 22:18:13 2020    NTUSER.DAT{016888bd-6c6f-11de-8d1d-001e0bcde3ec}.TMContainer00000000000000000002.regtrans-ms
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Pictures
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Saved Games
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Videos
        .\Users\Default\AppData\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Local
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Roaming
        .\Users\Default\AppData\Local\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Microsoft
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Temp
        .\Users\Default\AppData\Local\Microsoft\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Windows
        .\Users\Default\AppData\Local\Microsoft\Windows\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    GameExplorer
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    History
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Temporary Internet Files
        .\Users\Default\AppData\Roaming\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Media Center Programs
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Microsoft
        .\Users\Default\AppData\Roaming\Microsoft\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Internet Explorer
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Windows
        .\Users\Default\AppData\Roaming\Microsoft\Internet Explorer\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Quick Launch
        .\Users\Default\AppData\Roaming\Microsoft\Windows\*
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    .
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    ..
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Cookies
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Network Shortcuts
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Printer Shortcuts
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Recent
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    SendTo
        dw--w--w--                0 Sun Apr 19 14:51:00 2020    Start Menu
        dr--r--r--                0 Sun Apr 19 14:51:00 2020    Templates
        .\Users\Share\*
        dr--r--r--                0 Thu May 14 20:58:07 2020    .
        dr--r--r--                0 Thu May 14 20:58:07 2020    ..
        fr--r--r--            13312 Thu May 14 20:58:07 2020    gatekeeper.exe
```
		
It looks like we found the gatekeeper. Guess I'll get to use the Windows 11 dev box again.

## Exploit development

I moved the file over to my dev box and fired it up in the debugger.

