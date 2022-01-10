# Connection info
domain: bandit.labs.overthewire.org
port: 2220

# bandit 0
pass: boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme 
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
bandit0@bandit:~$ 
```
`boJ9jbbUNNfktd78OOpsqOltutMc3MY1`
# bandit 1
```
andit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./- 
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
bandit1@bandit:~$ 
```
`CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9`
# bandit 2
```
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename 
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
bandit2@bandit:~$ 
```
`UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK`
# bandit 3
```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cat inhere/.hidden 
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
bandit3@bandit:~$ 
```
`pIwrPrtPN36QITSp3EQaw936yaFoFgAB`
# Bandit 4
![[ca1914085ee043ea97493ca709e07e6b.png]]

# Bandit 5
```
bandit5@bandit:~$ for x in $files; do cat $x; echo ""; done
C4oCI4hsDhJXkVK38XQDBsFacRrdQSveOod1nrOn9O4WOitoKbKRazQZ5ViMKsQibB67knrvrCUcP6Uz46SGSPdv0gLCbsDYjNVmB2sLh4F5edgZrS6sFI3ivFQsukcaGMVhaDP2wWE5UOT7fctx24acwUZMMNwMNefwXNPVxHemtnLUmPTLrousgRM2NjVqKDh78N7mdCCHKu8eJhOJWerVQNktHCrGfJryLh2BGvwpwv2Kp4wQEjIXSHL8diXLkRycMUt1ztaAQ2TNB9r3FOTgUvvvVB1

cat: inhere/maybehere08/spaces: No such file or directory

cat: file1: No such file or directory

sdNTKUpr4diMbc9Sk9avriRhEPmwnVKyUgmtdYuDS04hLwVXHqmmnlFr5MIu6KVZOPCliDasxGii8RuOKUIv0FIa9sNSfNGoW1Z3yK2W86bpWwNlHfwcSs1rkJgp3ZutpG4NGhez9DyQbfO3aK69rlU0PrZ9VCiowB6mJEVbVk78S1RtwNpepFEBvBQDbIfFzqvzf1xszbhXA8qEADKxmRQ7lLZNRHYhJbre2XYYNLI59AyXwx6AjC9xe9

cat: inhere/maybehere15/spaces: No such file or directory

cat: file2: No such file or directory

eKqR22ODhv7SlpxkAEJXSL7s8KT9EaaM3saDwVnXMajkCQfGuDjJZjKtBPMWAZGsDntoXUTxPLh9

bandit5@bandit:~$ 
```
`eKqR22ODhv7SlpxkAEJXSL7s8KT9EaaM3saDwVnXMajkCQfGuDjJZjKtBPMWAZGsDntoXUTxPLh9` ?
NOPE