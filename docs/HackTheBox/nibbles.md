# Nibbles

## Recon
Port scan shows an Apache webserver running on port 80.
Going to the site reveals a static HTML page. In the HTML comments there is a reference to the `/nibbleblog/` site.  

Ran feroxbuster agains this directory and found `/admin/` as wel as `/admin.php`. The second is a login page.

## Getting help
Got stuck here, I tried SQL injections, bruteforce and other tactics, but the password is the name of the box.

> User: `admin`
> pass: `nibbles`

## Exploitation
Searching for nibbles in metsploit is the follwing module: `exploit/multi/http/nibbleblog_file_upload`
This exploit matches our target application's version. I provided credentials achieved inthe last step which gave a shell as a normal user. I was able to get user.txt

The host is running Ubuntu 16.04.3. When searched in metasploit there is the following module: `exploit/linux/local/bpf_sign_extension_priv_esc` which gave root. I was able to get root.txt