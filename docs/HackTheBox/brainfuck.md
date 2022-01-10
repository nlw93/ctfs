NGINX is running on port 443 but could not access the website by IP.
After adding a name resolution to `/etc/hosts` I was able to access the site at `https://brainfuck.htb`

It's running WordPress 4.7.3, there is a login page at `/wp-admin`

One of the plugins is vulnerable
```bash
┌──(nate@kali-Dell)-[~/…/active/brainfuck/results/exploit]                                                                                                                   └─$ searchsploit ticket system 7.1.3                                                                                                                                         ------------------------------------------------------------------------------------------------------------------------------------------- --------------------------------- Exploit Title                                                                                                                             |  Path                           ------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------WordPress Plugin WP Support Plus Responsive Ticket System 7.1.3 - Privilege Escalation                                                     | php/webapps/41006.txt           WordPress Plugin WP Support Plus Responsive Ticket System 7.1.3 - SQL Injection                                                            | php/webapps/40939.txt           ------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------Shellcodes: No Results                                                                                                                                                       
```

EDB `41006`, `40939`
- These vulnerabilities require user.


Nikto picked up an interesting link in the response headers:
```
+ Uncommon header 'link' found, with contents: <https://brainfuck.htb/?rest_route=/>; rel="https://api.w.org/"
```

Enumerating the API shows a bunch of different endpoints exposed.
One of the endpoints displays all users - the only user in this case is admin.

![[Pasted image 20220105072643.png]]

Maybe I can inject a user?
![[Pasted image 20220105073120.png]]

NOTE - brainfuck (the name of this box) is also the name of an encoding algorithm on multi-decoder (cache slueth). I'm working on something else right now so can't dig into it but this might be the right path for this box.