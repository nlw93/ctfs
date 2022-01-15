# *Brainf*ck

`*Still in progress`

## Recon

The tags for [this box](https://app.hackthebox.com/machines/17) are:  
> - Cryptography

The difficulty is marked as  
> - Insane


NGINX is running on port 443 but could not access the website by IP.
After adding a name resolution to `/etc/hosts` I was able to access the site at `https://brainfuck.htb`. It's running WordPress 4.7.3, there is a login page at `/wp-admin`

One of the plugins is vulnerable
```bash
searchsploit ticket system 7.1.3                                         
```

![[Pasted image 20220115091350.png]]

EDB `41006`, `40939`
- These vulnerabilities require user-level access, so they aren't useful yet.

Nikto picked up an interesting link in the response headers:
```
+ Uncommon header 'link' found, with contents: <https://brainfuck.htb/?rest_route=/>; rel="https://api.w.org/"
```

Enumerating the API shows a bunch of different endpoints exposed.
One of the endpoints displays all users - the only user in this case is admin.

![[Pasted image 20220105072643.png]]

Maybe I can inject a user?
![[Pasted image 20220105073120.png]]

> NOTE - brainfuck (the name of this box) is also the name of an encoding algorithm on [this decoder](https://www.cachesleuth.com/multidecoder/). 

At this point, it seems I should be looking for some brainfuck-encoded value and try to decode it.