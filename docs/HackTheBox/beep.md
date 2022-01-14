# Beep

## Recon

Tags: LFI, web 

### Scan findings
```nmap
PORT      STATE SERVICE    REASON         VERSION
22/tcp    open  ssh        syn-ack ttl 63 OpenSSH 4.3 (protocol 2.0)
25/tcp    open  smtp       syn-ack ttl 63 Postfix smtpd
|_smtp-commands: beep.localdomain, PIPELINING, SIZE 10240000, VRFY, ETRN, ENHANCEDSTATUSCODES, 8BITMIME, DSN
80/tcp    open  http       syn-ack ttl 63 Apache httpd 2.2.3
|_http-server-header: Apache/2.2.3 (CentOS)
|_http-title: Did not follow redirect to https://10.10.10.7/
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
110/tcp   open  pop3       syn-ack ttl 63 Cyrus pop3d 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
111/tcp   open  rpcbind    syn-ack ttl 63 2 (RPC #100000)
| rpcinfo:
|   program version    port/proto  service
|   100000  2            111/tcp   rpcbind
|   100000  2            111/udp   rpcbind
|   100024  1            875/udp   status
|_  100024  1            878/tcp   status
143/tcp   open  imap       syn-ack ttl 63 Cyrus imapd 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4
|_imap-capabilities: NAMESPACE THREAD=ORDEREDSUBJECT LIST-SUBSCRIBED URLAUTHA0001 LISTEXT OK X-NETSCAPE IDLE RENAME CATENATE CONDSTORE MULTIAPPEND THREAD=REFERENCES ANNOTATEMORE Completed SORT=MODSEQ SORT IMAP4rev1 ATOMIC NO QUOTA BINARY LITERAL+ ACL CHILDREN ID IMAP4 STARTTLS MAILBOX-REFERRALS UNSELECT UIDPLUS RIGHTS=kxte
443/tcp   open  ssl/http   syn-ack ttl 63 Apache httpd 2.2.3 ((CentOS))
|_http-server-header: Apache/2.2.3 (CentOS)
| http-robots.txt: 1 disallowed entry
|_/
878/tcp   open  status     syn-ack ttl 63 1 (RPC #100024)
993/tcp   open  ssl/imap   syn-ack ttl 63 Cyrus imapd
995/tcp   open  pop3       syn-ack ttl 63 Cyrus pop3d
3306/tcp  open  mysql      syn-ack ttl 63 MySQL (unauthorized)
|_tls-nextprotoneg: ERROR: Script execution failed (use -d to debug)
|_ssl-cert: ERROR: Script execution failed (use -d to debug)
|_tls-alpn: ERROR: Script execution failed (use -d to debug)
|_ssl-date: ERROR: Script execution failed (use -d to debug)
|_sslv2: ERROR: Script execution failed (use -d to debug)
4190/tcp  open  sieve      syn-ack ttl 63 Cyrus timsieved 2.3.7-Invoca-RPM-2.3.7-7.el5_6.4 (included w/cyrus imap)
4445/tcp  open  upnotifyp? syn-ack ttl 63
4559/tcp  open  hylafax    syn-ack ttl 63 HylaFAX 4.3.10
5038/tcp  open  asterisk   syn-ack ttl 63 Asterisk Call Manager 1.1
10000/tcp open  http       syn-ack ttl 63 MiniServ 1.570 (Webmin httpd)
|_http-trane-info: Problem with XML parsing of /evox/about
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html; Charset=iso-8859-1).
Aggressive OS guesses: Linux 2.6.9 - 2.6.24 (95%),
```

Some users found on port 25
```
[25][smtp-enum] host: 10.10.10.7   login: info  
[25][smtp-enum] host: 10.10.10.7   login: adm  
[25][smtp-enum] host: 10.10.10.7   login: root  
[25][smtp-enum] host: 10.10.10.7   login: mysql  
[25][smtp-enum] host: 10.10.10.7   login: ftp
```

https webserver has a few webpages
```
200        1l        6w      894c https://10.10.10.7/favicon.ico  
200        2l        4w       28c https://10.10.10.7/robots.txt  
200       35l      111w     1785c https://10.10.10.7/  
200       35l      111w     1785c https://10.10.10.7/config.php  
200       35l      111w     1785c https://10.10.10.7/index.php  
200       35l      111w     1785c https://10.10.10.7/register.php
```

The site on 443 is an Elastics CRM login page.

![[Pasted image 20220113213548.png]]

## Exploitation

It might be this, considering the LFI tag.

![[Pasted image 20220113212032.png]]

That exploit doesn't work, however it does help to enumerate vtigercrm. I checked, and sure enough it's on this box. The login page even discloses the version `5.1.0`

![[Pasted image 20220113213050.png]]

This new exploit [EDB-18770](https://www.exploit-db.com/exploits/18770), suggests the following url: `https://localhost/vtigercrm/modules/com_vtiger_workflow/sortfieldsjson.php?module_name=../../../../../../../../etc/passwd%00`

Sure enough, I can read files now.

![[Pasted image 20220113213426.png]]


