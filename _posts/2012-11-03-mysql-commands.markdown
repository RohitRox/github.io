---
layout: post
title: "MySql commands"
date: 2012-11-03 14:26
comments: true
categories: [MySQL]
---
This is a short-list of handy MySQL commands that I have to use time and time again, in my development machine as well as in servers and for importing and exporting databases.

Login to mysql console (from unix bash)
```bash
	$ mysql -u user -p
	# if host name is required
	$ mysql -h hostname -u user -p

```

Create a new database
``` bash
	mysql> create database databasename;
```

List all databases
``` bash
	mysql> show databases;
```

<!--more-->

Switch to a database
``` bash
	mysql> use database_name;
```

List tables
``` bash
	mysql> show tables;
```

See database's field formats.
``` bash
	 mysql> describe table_name;
```

Drop a db.
``` bash
	mysql> drop database database_name;
```

Delete a table.
``` bash
	mysql> drop table table_name;
```

Show all data in a table.
``` bash
	mysql> SELECT * FROM table_name;
```

Returns the columns and column information pertaining to the designated table.
``` bash
	mysql> show columns from table_name;
```

Set a root password if there is on root password.
``` bash
	$ mysqladmin -u root password newpassword
```

Update a root password.
``` bash
	$ mysqladmin -u root -p oldpassword newpassword
```

Recover a MySQL root password. 
Stop the MySQL server process. Start again with no grant tables. Login to MySQL as root. Set new password. Exit MySQL and restart MySQL server.
``` bash
	$ /etc/init.d/mysql stop
	$ mysqld_safe --skip-grant-tables &
	$ mysql -u root
	mysql> use mysql;
	mysql> update user set password=PASSWORD("newrootpassword") where User='root';
	mysql> flush privileges;
	mysql> quit
	$ /etc/init.d/mysql stop
	$ /etc/init.d/mysql start
```
Export a database for backup.
``` bash
	$ mysqldump -u user -p database_name > file.sql
```

Import a database
``` bash
	$ mysql -p -u username database_name < file.sql 
```
Dump all databases
``` bash
	$ mysqldump -u root -password --opt > alldatabases.sql
```

