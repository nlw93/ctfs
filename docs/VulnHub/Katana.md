# Katana

on port 80 there is a webserver running with bookstore CMS

EDB-48960 and EDB-49314 detail a few SQL injections that apply to this webserver. I tried to exploit them manually and was able to login as admin (the password is just admin anyhow.). I was not able to do anyhting else with manual exploitation.

SQLmap was able to dump the db, but I haven't been albe to get a shell with it.

```
sqlmap -u http://192.168.1.119/ebook/bookPerPub.php?pubid=4 -p 'pubid' --dbms=mariadb --dump --os-shell
```

