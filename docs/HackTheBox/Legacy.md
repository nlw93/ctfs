# Legacy

## Recon

Tags for [this box](https://app.hackthebox.com/machines/2) are:
> - Windows
> - Injection

Difficulty:
> Easy  


A port scan on shows only 2 open TCP ports: `139`, and `445`. Nmap guessed the OS was Windows XP or similar.

```bash
 PORT     STATE  SERVICE       REASON          VERSION  
 139/tcp  open   netbios-ssn   syn-ack ttl 127 Microsoft Windows netbios-ssn  
 445/tcp  open   microsoft-ds  syn-ack ttl 127 Windows XP microsoft-ds  
 3389/tcp closed ms-wbt-server reset ttl 127

Aggressive OS guesses: Microsoft Windows XP SP3 (94%)
```

A deeper look at port 139 reveals this host has not patched **MS17-010** (a.k.a. Eternal Blue)

```bash
| smb-vuln-ms17-010:
|   VULNERABLE:
|   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
|     State: VULNERABLE
|     IDs:  CVE:CVE-2017-0143
|     Risk factor: HIGH
|       A critical remote code execution vulnerability exists in Microsoft SMBv1
|        servers (ms17-010).
|
|     Disclosure date: 2017-03-14
|     References:
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
|       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
|_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
```


## Exploitation
Since the path was carved so cleanly from the beginning, I chose to use the metasploit module for this one. It's good practice, right?

![[Pasted image 20220103224012.png]]

user: `e69af0e4f443de7e36876fda4ec7644f`  
root: `993442d258b0e0ec917cae9e695d5713`