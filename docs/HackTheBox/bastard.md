# Bastard

## Recon  

Port scan revealed webserver on port 80, RPC on 139 and 49154.
The webserver has Drupal 7 running on IIS 7.5

### Enumerating the webserver

The webserver is using the following tech:  

- IIS 7.5
- PHP 5.4.28
- Drupal 7
- ASP.NET

There are some interesting Disallows in `robots.txt`.

```bash
                                                                                                                                                              
┌──(nate@kali-Dell)-[~/ctfs/docs/HackTheBox]
└─$ tmux ls        
hackn: 2 windows (created Wed Jan  5 21:52:14 2022) (attached)
                                                                                                                                                              
┌──(nate@kali-Dell)-[~/ctfs/docs/HackTheBox]
└─$ tmux a -t hackn



































# Directories
Disallow: /includes/
Disallow: /misc/
Disallow: /modules/
Disallow: /profiles/
Disallow: /scripts/
Disallow: /themes/
# Files
Disallow: /CHANGELOG.txt
Disallow: /cron.php
Disallow: /INSTALL.mysql.txt
Disallow: /INSTALL.pgsql.txt
Disallow: /INSTALL.sqlite.txt
Disallow: /install.php
Disallow: /INSTALL.txt
Disallow: /LICENSE.txt
Disallow: /MAINTAINERS.txt
Disallow: /update.php
Disallow: /UPGRADE.txt
Disallow: /xmlrpc.php
# Paths (clean URLs)
Disallow: /admin/
Disallow: /comment/reply/
Disallow: /filter/tips/
Disallow: /node/add/
Disallow: /search/
Disallow: /user/register/
Disallow: /user/password/
Disallow: /user/login/
Disallow: /user/logout/
# Paths (no clean URLs)
Disallow: /?q=admin/
Disallow: /?q=comment/reply/
Disallow: /?q=filter/tips/
Disallow: /?q=node/add/
Disallow: /?q=search/
Disallow: /?q=user/password/
Disallow: /?q=user/register/
Disallow: /?q=user/login/
Disallow: /?q=user/logout/
```

#### IIS 7.5
#### Drupal 7

There are many exploits available for the various modules of drupal 7. I should try to get a better picture of the attack surface before picking one.
