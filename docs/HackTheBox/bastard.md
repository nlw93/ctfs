# Bastard

## Recon  

The tags: `Windows`, `PHP`, `Patch Management`, `Web`

Port scan revealed webserver on port 80, RPC on 139 and 49154.
The webserver has Drupal 7 running on IIS 7.5

### Enumerating the webserver

The webserver is using the following tech:  

- IIS 7.5  
	- Potentially vulnerable to: 19033
- PHP 5.4.28  
	- 35145, and many more.
- Drupal 7
	- Many, need more enumeration to see what applies.
- ASP.NET

There are some interesting Disallows in `robots.txt`.

```bash
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

There are also some interesting findings by feroxbuster

```bash
200      159l      413w     7583c http://10.10.10.9/0
200      159l      413w     7583c http://10.10.10.9/node
200        1l        7w       62c http://10.10.10.9/rest
200       90l      243w     2189c http://10.10.10.9/robots.txt
200      152l      394w     7420c http://10.10.10.9/user
200      159l      413w     7583c http://10.10.10.9/0
200      159l      413w     7583c http://10.10.10.9/index.php
200       45l      262w     1717c http://10.10.10.9/install.mysql.txt
200       44l      290w     1874c http://10.10.10.9/install.pgsql.txt
200      159l      413w     7583c http://10.10.10.9/node
200        1l        7w       62c http://10.10.10.9/rest
200       90l      243w     2189c http://10.10.10.9/robots.txt
200      152l      394w     7420c http://10.10.10.9/user
200        1l        6w       42c http://10.10.10.9/xmlrpc.php
200      152l      394w     7420c http://10.10.10.9/user
200      159l      413w     7583c http://10.10.10.9/node
200      159l      413w     7583c http://10.10.10.9/
200      159l      413w     7583c http://10.10.10.9/0
200      175l      510w     9063c http://10.10.10.9/User
200        1l        7w       62c http://10.10.10.9/rest
200        1l        7w       62c http://10.10.10.9/REST
200      175l      510w     9063c http://10.10.10.9/USER
200      123l      714w     5382c http://10.10.10.9/ReadMe.txt

```

# Exploitation

I'm leaning toward `/xmlrpc.php` as an attack surface. I've seen it vulnerable in the past.
