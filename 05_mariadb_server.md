# MariaDB Database Server

## Objectives

- Installing mariadb 
- Initial configuration
- Connecting to a local database using the mysql client
- Initial exploration
- Create database
- Drop database
- Create users
- Grant user privileges
- Drop users
- Connecting to a remote database
- Exiting the mysql cli


## Introduction

Structured query language (SQL) is a standard language for database creation and manipulation. MariaDB is a relational database program that uses SQL queries.

## Installing MariaDB

To install the Apache webserver, the `dnf install` command can be used as with any software installation in Linux.  Refer to the [lab](https://github.com/ChaoChingTan/labs/blob/main/Linux_Software_Installation.md) on software installation on how to install packages.  The package for MariaDB server is `mariadb-server`. 

```console
[student@ip-172-31-88-88 ~]$ sudo dnf install mariadb-server
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 1:23:55 ago on Fri 06 Oct 2023 01:38:42 AM UTC.
Dependencies resolved.
================================================================================
 Package              Arch   Version           Repository                  Size
================================================================================
Installing:
 mariadb-server       x86_64 3:10.5.16-2.el9_0 rhel-9-appstream-rhui-rpms 9.4 M
Installing dependencies:
 libaio               x86_64 0.3.111-13.el9    rhel-9-baseos-rhui-rpms     26 k
 mariadb              x86_64 3:10.5.16-2.el9_0 rhel-9-appstream-rhui-rpms 1.6 M
 mariadb-common       x86_64 3:10.5.16-2.el9_0 rhel-9-appstream-rhui-rpms  39 k
 mariadb-connector-c  x86_64 3.2.6-1.el9_0     rhel-9-appstream-rhui-rpms 203 k
 mariadb-connector-c-config
.
.
<Output Truncated>
.
.
  perl-mro-1.23-480.el9.x86_64                                                  
  perl-overload-1.31-480.el9.noarch                                             
  perl-overloading-0.02-480.el9.noarch                                          
  perl-parent-1:0.238-460.el9.noarch                                            
  perl-podlators-1:4.14-460.el9.noarch                                          
  perl-subs-1.03-480.el9.noarch                                                 
  perl-vars-1.05-480.el9.noarch                                                 

Complete!
[student@ip-172-31-88-88 ~]$ 
```

## Initial configuration

After installation, one of first tasks that needs to be done is the initial configuration of the MariaDB setup.  

Start the mariadb service using `systemctl start` and check the status of the service by using `systemctl status`.  Refer to the [chapter on services](https://github.com/ChaoChingTan/labs/blob/main/Linux_Services.md) if you require more information.  

```console
[student@ip-172-31-88-88 ~]$ sudo systemctl start mariadb
[student@ip-172-31-88-88 ~]$ sudo systemctl status mariadb
● mariadb.service - MariaDB 10.5 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled; preset:>
     Active: active (running) since Fri 2023-10-06 07:51:13 UTC; 9s ago
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 1886 ExecStartPre=/usr/libexec/mariadb-check-socket (code=exited, >
    Process: 1908 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir mariadb.serv>
    Process: 2001 ExecStartPost=/usr/libexec/mariadb-check-upgrade (code=exited>
   Main PID: 1989 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 11 (limit: 4421)
     Memory: 98.8M
        CPU: 350ms
     CGroup: /system.slice/mariadb.service
             └─1989 /usr/libexec/mariadbd --basedir=/usr

Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: you>
Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: Aft>
Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: abl>
Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: See>
Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: Ple>
Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: The>
Oct 06 07:51:13 ip-172-31-92-134.ec2.internal mariadb-prepare-db-dir[1947]: Con>
lines 1-23
```

Once the service is started, you can use the command `mysql_secure_installation` to perform the initial configuration.  Note that you will require root privilege for this.

```console
[student@ip-172-31-88-88 ~]$ sudo mysql_secure_installation 

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user. If you've just installed MariaDB, and
haven't set the root password yet, you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password or using the unix_socket ensures that nobody
can log into the MariaDB root user without the proper authorisation.

You already have your root account protected, so you can safely answer 'n'.

Switch to unix_socket authentication [Y/n] 
Enabled successfully!
Reloading privilege tables..
 ... Success!


You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] 
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] 
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] 
 ... Success!

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] 
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] 
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!
```

For the configurations, we used all defaults except for the database root user password.  **Note that this is different from the system root password**:

```console
You already have your root account protected, so you can safely answer 'n'.

Change the root password? [Y/n] 
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!
```

## Connecting to a local database using the mysql client

After setting the database root user password, you are now ready to connect to the database using the MySQL cli.  The MySQL cli is a text-based interface provided by MySQL for interacting with MySQL database servers directly from the command line.

Connect to your server by issuing the command:

```bash
mysql -u root -h 127.0.0.1 -p
```

The option `-u` is to specify the user (you can create different users with different privileges), `-h` is to specify the server to connect to (127.0.0.1 in the example, which is the default even without specifying), and the option `-p` is to prompt for the password.  

The output will look similar to:

```console
[student@ip-172-31-88-88 ~]$ mysql -u root -h 127.0.0.1 -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 18
Server version: 10.5.16-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```

## Initial exploration

The prompt below shows that you are in the mysql cli:

```console
MariaDB [(none)]>
```

At this point, you can issue SQL statements to your database.  To show the databases, issue the statement `show databases;`.  Note that SQL statements need to be terminated with a `;` for most cases.  

```console
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.001 sec)

MariaDB [(none)]> 
```

Select a specific database to work with using the `use` statement:

```console
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
```

Once you have selected a database to use, you can use the `show tables` statement to display a list of tables in the current database.

```console
MariaDB [mysql]> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| column_stats              |
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| global_priv               |
| gtid_slave_pos            |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| index_stats               |
| innodb_index_stats        |
| innodb_table_stats        |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| roles_mapping             |
| servers                   |
| slow_log                  |
| table_stats               |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| transaction_registry      |
| user                      |
+---------------------------+
31 rows in set (0.000 sec)

MariaDB [mysql]> 
```

The `select` statement can be used to retrieve data from one or more tables in a database:

```console
MariaDB [mysql]> select user from user;
+-------------+
| User        |
+-------------+
| mariadb.sys |
| mysql       |
| root        |
+-------------+
3 rows in set (0.001 sec)
```

The statement above shows all the users in the user table.  This table contain user details such as usernames and passwords.

## Create database

To create a new database on your MariaDB server, issue the `create database` statement:

```console
MariaDB [mysql]> create database mydb;
Query OK, 1 row affected (0.000 sec)

MariaDB [mysql]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydb               |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.000 sec)

MariaDB [mysql]> 
```

## Drop database

Drop database means to remove the database.  **BE CAREFUL WHEN YOU ISSUE THIS STATEMENT, THERE IS NO UNDO BUTTON**

```console
MariaDB [mysql]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydb               |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.000 sec)

MariaDB [mysql]> drop database mydb;
Query OK, 0 rows affected (0.001 sec)

MariaDB [mysql]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.000 sec)

MariaDB [mysql]> 
```

## Create users

The SQL statement to create users has the following sytax:

```console
CREATE USER 'USERNAME'@'HOST' IDENTIFIED BY 'PASSWORD';
```

Where:
- `USERNAME` is the database user to create
- `HOST` is the host which the user is allowed to connect from
- `PASSWORD` is the password of the created database user 


```console
MariaDB [mysql]> create user 'dbuser'@'localhost' identified by 'secret';
Query OK, 0 rows affected (0.001 sec)

MariaDB [mysql]> 
```

The example above creates a user *dbuser* who can only connect to the database server from *localhost*,  the same machine where the server is running.  User has a password of *secret*.


```console
MariaDB [mysql]> select user,host,password from user where user='dbuser';
+--------+-----------+-------------------------------------------+
| User   | Host      | Password                                  |
+--------+-----------+-------------------------------------------+
| dbuser | localhost | *14E65567ABDB5135D0CFD9A70B3032C179A49EE7 |
+--------+-----------+-------------------------------------------+
1 row in set (0.001 sec)

MariaDB [mysql]>
```

## Grant user privileges

After creating a user, it is usual to grant some privileges to the user.  This allows the user to perform certain actions on a specific database or globally:

```console
MariaDB [mysql]> grant all privileges on mydb.* to 'dbuser'@'localhost';
Query OK, 0 rows affected (0.001 sec)
```

To see the grants for a user, use the `show grants` statement

```console
MariaDB [mysql]> show grants for  'dbuser'@'localhost';
+---------------------------------------------------------------------------------------------------------------+
| Grants for dbuser@localhost                                                                                   |
+---------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `dbuser`@`localhost` IDENTIFIED BY PASSWORD '*14E65567ABDB5135D0CFD9A70B3032C179A49EE7' |
| GRANT ALL PRIVILEGES ON `mydb`.* TO `dbuser`@`localhost`                                                      |
+---------------------------------------------------------------------------------------------------------------+
2 rows in set (0.000 sec)
```

This grants privileges to all tables in `mydb` database but only usage on other databases.  To illustrate we will logout of the current user by issuing the `quit` statement and login as *dbuser*.

```console
MariaDB [mysql]> quit;
Bye
[student@ip-172-31-88-88 ~]$ mysql -u dbuser -h 127.0.0.1 -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 19
Server version: 10.5.16-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
+--------------------+
1 row in set (0.000 sec)

MariaDB [(none)]> create database mydb;
Query OK, 1 row affected (0.000 sec)

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mydb               |
+--------------------+
2 rows in set (0.000 sec)

MariaDB [(none)]> create database yourdb;
ERROR 1044 (42000): Access denied for user 'dbuser'@'localhost' to database 'yourdb'
MariaDB [(none)]> drop database mydb;
Query OK, 0 rows affected (0.002 sec)

MariaDB [(none)]> drop database information_schema;
ERROR 1044 (42000): Access denied for user 'dbuser'@'localhost' to database 'information_schema'
MariaDB [(none)]> 
```

## Drop users

To delete users from the database, issue the `drop user` statement.  

```console
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> select user from user;
+-------------+
| User        |
+-------------+
| dbuser      |
| mariadb.sys |
| mysql       |
| root        |
+-------------+
4 rows in set (0.001 sec)

MariaDB [mysql]> drop user 'dbuser'@'localhost';
Query OK, 0 rows affected (0.001 sec)

MariaDB [mysql]> select user from user;
+-------------+
| User        |
+-------------+
| mariadb.sys |
| mysql       |
| root        |
+-------------+
3 rows in set (0.001 sec)

MariaDB [mysql]> 
```

## Connecting to a remote database

Connecting to a remote database is achieved by using the mysql cli and specifying the remote host to connect to:

```console
mysql -u root -h HOSTNAME_OR_IPADDRESS -p
```

## Exiting the mysql cli

To exit the mysql cli, issue the statement `quit;`.  You will exit the cli and be returned to the command prompt. 

```console
MariaDB [mysql]> quit;
Bye
[student@ip-172-31-88-88 ~]$ 
```

## Lab

1. Install the maridb-server package.  Refer to the [chapter on software installation](https://github.com/ChaoChingTan/labs/blob/main/Linux_Software_Installation.md) for procedures.

2. Start **and enable** the mariadb service.  Refer to the [chapter on services](https://github.com/ChaoChingTan/labs/blob/main/Linux_Services.md) for procedures.  

3. Perform initial configuration of the MariaDB using the `mysql_secure_installation` command as root.

4. Connect to your MariaDB server using the mysql cli:

    - show the databases present

    - show the user table in the mysql database

5. Create a database called `wordpress`

6. Create a database user `wpuser` with password `wpsecret` and allow this user to connect to the database only from the `localhost`

7. Grant all privileges on `wordpress` database to the `wpuser` you created

8. Verify that you have performed the steps 5-7

9. Drop the user you have created.

10. Drop the database you have created. 


## Conclusion

In this chapter we have installed a MariaDB server, started and enabled the mariadb service.  We have investigated creating, deleting and reading database information from the command line.


## References

1. [MariaDB](https://mariadb.org/)