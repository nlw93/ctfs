# Bandit 0
Pass: bandit0
```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```
# Bandit 1
Pass: boJ9jbbUNNfktd78OOpsqOltutMc3MY1

```
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```
# Bandit 2
Pass: CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```
# Bandit 3
Pass: UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -alt
total 12
drwxr-xr-x 2 root    root    4096 May  7  2020 .
-rw-r----- 1 bandit4 bandit3   33 May  7  2020 .hidden
drwxr-xr-x 3 root    root    4096 May  7  2020 ..
bandit3@bandit:~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```
# Bandit4
Pass: pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ ls
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
bandit4@bandit:~/inhere$ which python
/usr/bin/python
bandit4@bandit:~/inhere$ which python3
/usr/bin/python3
bandit4@bandit:~/inhere$ sudo -l
sudo: /usr/bin/sudo must be owned by uid 0 and have the setuid bit set
bandit4@bandit:~/inhere$ file ./*
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

# Bandit 5
Pass: koReBOKuIDDepwhWk7jZC0RTdopnAYKh

```
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ cd inhere/
bandit5@bandit:~/inhere$ ls
maybehere00  maybehere05  maybehere10  maybehere15
maybehere01  maybehere06  maybehere11  maybehere16
maybehere02  maybehere07  maybehere12  maybehere17
maybehere03  maybehere08  maybehere13  maybehere18
maybehere04  maybehere09  maybehere14  maybehere19
bandit5@bandit:~/inhere$ ls ./*
./maybehere00:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere01:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere02:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere03:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere04:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere05:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere06:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere07:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere08:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere09:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere10:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere11:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere12:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere13:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere14:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere15:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere16:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere17:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere18:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3

./maybehere19:
-file1  -file3        spaces file2
-file2  spaces file1  spaces file3
bandit5@bandit:~/inhere$ file ./*/*
./maybehere00/-file1:       ASCII text, with very long lines
./maybehere00/-file2:       ASCII text, with very long lines
./maybehere00/-file3:       PGP\011Secret Key -
./maybehere00/spaces file1: ASCII text, with very long lines
./maybehere00/spaces file2: ASCII text, with very long lines
./maybehere00/spaces file3: data
./maybehere01/-file1:       ASCII text, with very long lines
./maybehere01/-file2:       ASCII text
./maybehere01/-file3:       data
./maybehere01/spaces file1: ASCII text, with very long lines
./maybehere01/spaces file2: ASCII text, with very long lines
./maybehere01/spaces file3: data
./maybehere02/-file1:       ASCII text, with very long lines
./maybehere02/-file2:       X1 archive data
./maybehere02/-file3:       data
./maybehere02/spaces file1: ASCII text, with very long lines
./maybehere02/spaces file2: ASCII text, with very long lines
./maybehere02/spaces file3: data
./maybehere03/-file1:       ASCII text, with very long lines
./maybehere03/-file2:       ASCII text, with very long lines
./maybehere03/-file3:       data
./maybehere03/spaces file1: ASCII text, with very long lines
./maybehere03/spaces file2: ASCII text, with very long lines
./maybehere03/spaces file3: data
./maybehere04/-file1:       ASCII text, with very long lines
./maybehere04/-file2:       ASCII text, with very long lines
./maybehere04/-file3:       data
./maybehere04/spaces file1: ASCII text, with very long lines
./maybehere04/spaces file2: ASCII text, with very long lines
./maybehere04/spaces file3: data
./maybehere05/-file1:       ASCII text, with very long lines
./maybehere05/-file2:       ASCII text, with very long lines
./maybehere05/-file3:       data
./maybehere05/spaces file1: ASCII text, with very long lines
./maybehere05/spaces file2: ASCII text, with very long lines
./maybehere05/spaces file3: data
./maybehere06/-file1:       ASCII text, with very long lines
./maybehere06/-file2:       ASCII text, with very long lines
./maybehere06/-file3:       data
./maybehere06/spaces file1: ASCII text, with very long lines
./maybehere06/spaces file2: ASCII text, with very long lines
./maybehere06/spaces file3: data
./maybehere07/-file1:       ASCII text, with very long lines
./maybehere07/-file2:       ASCII text, with very long lines
./maybehere07/-file3:       data
./maybehere07/spaces file1: ASCII text, with very long lines
./maybehere07/spaces file2: ASCII text, with very long lines
./maybehere07/spaces file3: data
./maybehere08/-file1:       ASCII text, with very long lines
./maybehere08/-file2:       ASCII text, with very long lines
./maybehere08/-file3:       data
./maybehere08/spaces file1: ASCII text
./maybehere08/spaces file2: ASCII text, with very long lines
./maybehere08/spaces file3: data
./maybehere09/-file1:       ASCII text, with very long lines
./maybehere09/-file2:       ASCII text, with very long lines
./maybehere09/-file3:       data
./maybehere09/spaces file1: ASCII text, with very long lines
./maybehere09/spaces file2: ASCII text, with very long lines
./maybehere09/spaces file3: data
./maybehere10/-file1:       ASCII text, with very long lines
./maybehere10/-file2:       ASCII text, with very long lines
./maybehere10/-file3:       data
./maybehere10/spaces file1: ASCII text, with very long lines
./maybehere10/spaces file2: ASCII text, with very long lines
./maybehere10/spaces file3: data
./maybehere11/-file1:       ASCII text, with very long lines
./maybehere11/-file2:       ASCII text, with very long lines
./maybehere11/-file3:       data
./maybehere11/spaces file1: ASCII text, with very long lines
./maybehere11/spaces file2: ASCII text, with very long lines
./maybehere11/spaces file3: data
./maybehere12/-file1:       ASCII text, with very long lines
./maybehere12/-file2:       ASCII text
./maybehere12/-file3:       data
./maybehere12/spaces file1: ASCII text, with very long lines
./maybehere12/spaces file2: ASCII text, with very long lines
./maybehere12/spaces file3: data
./maybehere13/-file1:       ASCII text, with very long lines
./maybehere13/-file2:       ASCII text, with very long lines
./maybehere13/-file3:       data
./maybehere13/spaces file1: ASCII text, with very long lines
./maybehere13/spaces file2: ASCII text, with very long lines
./maybehere13/spaces file3: data
./maybehere14/-file1:       ASCII text, with very long lines
./maybehere14/-file2:       ASCII text, with very long lines
./maybehere14/-file3:       data
./maybehere14/spaces file1: ASCII text, with very long lines
./maybehere14/spaces file2: ASCII text, with very long lines
./maybehere14/spaces file3: data
./maybehere15/-file1:       ASCII text, with very long lines
./maybehere15/-file2:       ASCII text, with very long lines
./maybehere15/-file3:       data
./maybehere15/spaces file1: ASCII text, with very long lines
./maybehere15/spaces file2: ASCII text
./maybehere15/spaces file3: data
./maybehere16/-file1:       ASCII text, with very long lines
./maybehere16/-file2:       ASCII text, with very long lines
./maybehere16/-file3:       data
./maybehere16/spaces file1: ASCII text, with very long lines
./maybehere16/spaces file2: ASCII text, with very long lines
./maybehere16/spaces file3: data
./maybehere17/-file1:       ASCII text, with very long lines
./maybehere17/-file2:       ASCII text, with very long lines
./maybehere17/-file3:       data
./maybehere17/spaces file1: ASCII text, with very long lines
./maybehere17/spaces file2: ASCII text, with very long lines
./maybehere17/spaces file3: data
./maybehere18/-file1:       ASCII text, with very long lines
./maybehere18/-file2:       ASCII text
./maybehere18/-file3:       data
./maybehere18/spaces file1: ASCII text, with very long lines
./maybehere18/spaces file2: ASCII text, with very long lines
./maybehere18/spaces file3: data
./maybehere19/-file1:       ASCII text, with very long lines
./maybehere19/-file2:       ASCII text, with very long lines
./maybehere19/-file3:       data
./maybehere19/spaces file1: ASCII text, with very long lines
./maybehere19/spaces file2: ASCII text, with very long lines
./maybehere19/spaces file3: data

```
## interesting finds
./maybehere00/-file3:       PGP\011Secret Key -
./maybehere01/-file2:       ASCII text
./maybehere02/-file2:       X1 archive data
./maybehere08/spaces file1: ASCII text
./maybehere12/-file2:       ASCII text
./maybehere15/spaces file2: ASCII text
./maybehere18/-file2:       ASCII text

### these all seem too large
```
bandit5@bandit:~/inhere$ ls -alt ./*/-file2
-rw-r----- 1 root bandit5 9499 May  7  2020 ./maybehere15/-file2
-rw-r----- 1 root bandit5 5301 May  7  2020 ./maybehere16/-file2
-rw-r----- 1 root bandit5 1791 May  7  2020 ./maybehere17/-file2
-rw-r----- 1 root bandit5   77 May  7  2020 ./maybehere18/-file2
-rw-r----- 1 root bandit5 5594 May  7  2020 ./maybehere19/-file2
-rw-r----- 1 root bandit5 4559 May  7  2020 ./maybehere11/-file2
-rw-r----- 1 root bandit5  251 May  7  2020 ./maybehere12/-file2
-rw-r----- 1 root bandit5 1423 May  7  2020 ./maybehere13/-file2
-rw-r----- 1 root bandit5 8351 May  7  2020 ./maybehere14/-file2
-rw-r----- 1 root bandit5 1076 May  7  2020 ./maybehere06/-file2
-rw-r----- 1 root bandit5 2488 May  7  2020 ./maybehere07/-file2
-rw-r----- 1 root bandit5 3825 May  7  2020 ./maybehere08/-file2
-rw-r----- 1 root bandit5  774 May  7  2020 ./maybehere09/-file2
-rw-r----- 1 root bandit5 1991 May  7  2020 ./maybehere10/-file2
-rw-r----- 1 root bandit5 6595 May  7  2020 ./maybehere03/-file2
-rw-r----- 1 root bandit5 2619 May  7  2020 ./maybehere04/-file2
-rw-r----- 1 root bandit5 5959 May  7  2020 ./maybehere05/-file2
-rw-r----- 1 root bandit5 9388 May  7  2020 ./maybehere00/-file2
-rw-r----- 1 root bandit5  288 May  7  2020 ./maybehere01/-file2
-rw-r----- 1 root bandit5 3511 May  7  2020 ./maybehere02/-file2
```