Port scan revealed only a webserver running on port 80.
The OS is windows, and the webserver application is HTTPFileServer 2.3 which is vulnerable to EDB 49125
https://www.exploit-db.com/exploits/49125

This exploit grants RCE and I was able to get a reverse shell by modifying the LHOST and LPORT and use the same `mini-shell.ps1` that's used in the exploit comments.
https://gist.github.com/staaldraad/204928a6004e89553a8d3db0ce527fd5

This grants a shell as `optimum\kostas` and I was able to capture user.txt.txt