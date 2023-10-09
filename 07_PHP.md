# PHP (Hypertext Preprocessor)

## Objectives

- [Installation](#installation)
- [PHP Hello World](#php-hello-world)
- [phpinfo()](#phpinfo)
- [Connecting to MariaDB using PHP](#connecting-to-mariadb-using-php)

## Introduction

PHP (Hypertext Preprocessor) is a popular open-source programming language that is ideal for web development. It is frequently used in conjunction with other technologies to build dynamic and interactive webpages, such as HTML, CSS, and JavaScript.

In this lab we will install PHP on a web server and demonstrate running PHP code by using the function *phpinfo()*.  

## Installation

Install the following pre-requisites:

1. Apache httpd server - For more details refer to the chapter on [Apache HTTP server](04_Apache_HTTP_server.md)

2. MariaDB database server - For more details refer to the chapter on [MariaDB database server](05_mariadb_server.md)

3. Install the following packages:

    i. php

    ii. php-cli

    iii. php-mysqlnd

```console
[student@ip-172-31-88-88 ~]$ sudo dnf install php php-mod_php php-mysql
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 6:41:14 ago on Sun 08 Oct 2023 06:55:32 PM UTC.
No match for argument: php-mod_php
No match for argument: php-mysql
Error: Unable to find a match: php-mod_php php-mysql
[student@ip-172-31-88-88 ~]$ sudo dnf install php php-cli php-mysqlnd
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 6:46:43 ago on Sun 08 Oct 2023 06:55:32 PM UTC.
Dependencies resolved.
========================================================================================================================================
 Package                         Architecture          Version                          Repository                                 Size
========================================================================================================================================
Installing:
 php                             x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                 11 k
 php-cli                         x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                3.1 M
 php-mysqlnd                     x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                154 k
Installing dependencies:
 libxslt                         x86_64                1.1.34-9.el9                     rhel-9-appstream-rhui-rpms                247 k
 nginx-filesystem                noarch                1:1.20.1-14.el9                  rhel-9-appstream-rhui-rpms                 13 k
 oniguruma                       x86_64                6.9.6-1.el9.5                    rhel-9-appstream-rhui-rpms                221 k
 php-common                      x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                686 k
 php-pdo                         x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                 87 k
Installing weak dependencies:
 php-fpm                         x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                1.6 M
 php-mbstring                    x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                473 k
 php-opcache                     x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                514 k
 php-xml                         x86_64                8.0.27-1.el9_1                   rhel-9-appstream-rhui-rpms                138 k

Transaction Summary
========================================================================================================================================
Install  12 Packages

Total download size: 7.2 M
Installed size: 37 M
Is this ok [y/N]: y
Downloading Packages:
(1/12): php-mysqlnd-8.0.27-1.el9_1.x86_64.rpm                                                           2.3 MB/s | 154 kB     00:00    
(2/12): oniguruma-6.9.6-1.el9.5.x86_64.rpm                                                              3.0 MB/s | 221 kB     00:00    
(3/12): libxslt-1.1.34-9.el9.x86_64.rpm                                                                 3.1 MB/s | 247 kB     00:00    
(4/12): php-8.0.27-1.el9_1.x86_64.rpm                                                                   941 kB/s |  11 kB     00:00    
(5/12): php-xml-8.0.27-1.el9_1.x86_64.rpm                                                               4.2 MB/s | 138 kB     00:00    
(6/12): php-fpm-8.0.27-1.el9_1.x86_64.rpm                                                                31 MB/s | 1.6 MB     00:00    
(7/12): php-mbstring-8.0.27-1.el9_1.x86_64.rpm                                                           17 MB/s | 473 kB     00:00    
(8/12): php-opcache-8.0.27-1.el9_1.x86_64.rpm                                                            21 MB/s | 514 kB     00:00    
(9/12): php-cli-8.0.27-1.el9_1.x86_64.rpm                                                                33 MB/s | 3.1 MB     00:00    
(10/12): php-pdo-8.0.27-1.el9_1.x86_64.rpm                                                              3.1 MB/s |  87 kB     00:00    
(11/12): php-common-8.0.27-1.el9_1.x86_64.rpm                                                            14 MB/s | 686 kB     00:00    
(12/12): nginx-filesystem-1.20.1-14.el9.noarch.rpm                                                      897 kB/s |  13 kB     00:00    
----------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                    29 MB/s | 7.2 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                                                                                1/1 
  Installing       : php-common-8.0.27-1.el9_1.x86_64                                                                              1/12 
  Installing       : php-pdo-8.0.27-1.el9_1.x86_64                                                                                 2/12 
  Installing       : php-cli-8.0.27-1.el9_1.x86_64                                                                                 3/12 
  Installing       : php-opcache-8.0.27-1.el9_1.x86_64                                                                             4/12 
  Running scriptlet: nginx-filesystem-1:1.20.1-14.el9.noarch                                                                       5/12 
  Installing       : nginx-filesystem-1:1.20.1-14.el9.noarch                                                                       5/12 
  Installing       : php-fpm-8.0.27-1.el9_1.x86_64                                                                                 6/12 
  Running scriptlet: php-fpm-8.0.27-1.el9_1.x86_64                                                                                 6/12 
  Installing       : oniguruma-6.9.6-1.el9.5.x86_64                                                                                7/12 
  Installing       : php-mbstring-8.0.27-1.el9_1.x86_64                                                                            8/12 
  Installing       : libxslt-1.1.34-9.el9.x86_64                                                                                   9/12 
  Installing       : php-xml-8.0.27-1.el9_1.x86_64                                                                                10/12 
  Installing       : php-8.0.27-1.el9_1.x86_64                                                                                    11/12 
  Installing       : php-mysqlnd-8.0.27-1.el9_1.x86_64                                                                            12/12 
  Running scriptlet: php-mysqlnd-8.0.27-1.el9_1.x86_64                                                                            12/12 
  Verifying        : libxslt-1.1.34-9.el9.x86_64                                                                                   1/12 
  Verifying        : oniguruma-6.9.6-1.el9.5.x86_64                                                                                2/12 
  Verifying        : php-mysqlnd-8.0.27-1.el9_1.x86_64                                                                             3/12 
  Verifying        : php-8.0.27-1.el9_1.x86_64                                                                                     4/12 
  Verifying        : php-fpm-8.0.27-1.el9_1.x86_64                                                                                 5/12 
  Verifying        : php-cli-8.0.27-1.el9_1.x86_64                                                                                 6/12 
  Verifying        : php-xml-8.0.27-1.el9_1.x86_64                                                                                 7/12 
  Verifying        : php-mbstring-8.0.27-1.el9_1.x86_64                                                                            8/12 
  Verifying        : php-opcache-8.0.27-1.el9_1.x86_64                                                                             9/12 
  Verifying        : php-common-8.0.27-1.el9_1.x86_64                                                                             10/12 
  Verifying        : php-pdo-8.0.27-1.el9_1.x86_64                                                                                11/12 
  Verifying        : nginx-filesystem-1:1.20.1-14.el9.noarch                                                                      12/12 
Installed products updated.

Installed:
  libxslt-1.1.34-9.el9.x86_64                nginx-filesystem-1:1.20.1-14.el9.noarch          oniguruma-6.9.6-1.el9.5.x86_64            
  php-8.0.27-1.el9_1.x86_64                  php-cli-8.0.27-1.el9_1.x86_64                    php-common-8.0.27-1.el9_1.x86_64          
  php-fpm-8.0.27-1.el9_1.x86_64              php-mbstring-8.0.27-1.el9_1.x86_64               php-mysqlnd-8.0.27-1.el9_1.x86_64         
  php-opcache-8.0.27-1.el9_1.x86_64          php-pdo-8.0.27-1.el9_1.x86_64                    php-xml-8.0.27-1.el9_1.x86_64             

Complete!    
```

Verify the php installation:

```console
[student@ip-172-31-88-88 ~]$ php -v
PHP 8.0.27 (cli) (built: Jan  3 2023 16:17:26) ( NTS gcc x86_64 )
Copyright (c) The PHP Group
Zend Engine v4.0.27, Copyright (c) Zend Technologies
    with Zend OPcache v8.0.27, Copyright (c), by Zend Technologies
```

## PHP Hello World

A simple PHP code to print out `Hello World!` with a carriage return at the end:

```bash
<?php echo "Hello World!\n";?>
```

Edit the file in your home directory and execute the file by runinng `php <FILENAME>`

```console
[student@ip-172-31-88-88 ~]$ vi helloWorld.php
[student@ip-172-31-88-88 ~]$ php helloWorld.php 
Hello World!
```

You can also make the file executable (refer to the chapter on [File Access Control](02_File_Access_Control.md) for more details).  After which you can execute the file by calling the file name.  You first need to modify the file to add the interpreter to use during execution:

```console
#!/usr/bin/php
<?php echo "Hello World!\n"; ?>
```

Changing the permission and executing the file:

```console
[student@ip-172-31-88-88 ~]$ ls -l helloWorld.php 
-rw-r--r--. 1 student student 47 Oct  9 01:55 helloWorld.php
[student@ip-172-31-88-88 ~]$ chmod a+x helloWorld.php 
[student@ip-172-31-88-88 ~]$ ls -l helloWorld.php 
-rwxr-xr-x. 1 student student 47 Oct  9 01:55 helloWorld.php
[student@ip-172-31-88-88 ~]$ ./helloWorld.php 
Hello World!
[student@ip-172-31-88-88 ~]$ 
```

## phpinfo()

The function phpinfo() outputs a large amount of information about the current state of PHP. phpinfo() is commonly used to check configuration settings and for available predefined variables on a given system.

To demonstrate phpinfo(), create a file called `phpinfo.php` with the content:

```console
<?php phpinfo(); ?>
```

Then execute it by running:

```bash
php phpinfo.php
```
You will see a lot of output on the screen.  The output can be rendered as HTML on screen, served as a webpage by the HTTP server.

Make the file `phpinfo.php` executable:

```console
[student@ip-172-31-88-88 ~]$ ls -l phpinfo.php 
-rw-r--r--. 1 student student 35 Oct  9 02:52 phpinfo.php
[student@ip-172-31-88-88 ~]$ chmod a+x phpinfo.php 
[student@ip-172-31-88-88 ~]$ ls -l phpinfo.php 
-rwxr-xr-x. 1 student student 35 Oct  9 02:52 phpinfo.php
```

Copy this file to the *DocumentRoot* of your httpd server, the default location is at `/var/www/html`.  Note that root privileges is required:

```console
[student@ip-172-31-88-88 ~]$ sudo cp phpinfo.php /var/www/html/
[student@ip-172-31-88-88 ~]$ ls -l /var/www/html/
total 4
-rwxr-xr-x. 1 root root 35 Oct  9 02:55 phpinfo.php
```

If your http server is already running, you will need to restart the httpd after php installation.  Issue the following command:

```bash
sudo systemctl restart httpd
```

As usual, check the httpd status after every start/stop/restart to ensure everything is working fine:

```bash
sudo systemctl status httpd
```

Access this page from the web browser, you will see a webpage with details about your php installation.  


## Connecting to MariaDB using PHP

We can also connect to a database server using PHP. The code below illustrates this:

```php
<?php

$host = "your_mysql_host";       // Replace with your MySQL host (e.g., "localhost")
$username = "your_mysql_username";   // Replace with your MySQL username
$password = "your_mysql_password";   // Replace with your MySQL password
$database = "your_mysql_database";   // Replace with your MySQL database name

// Create a connection
$conn = new mysqli($host, $username, $password, $database);

// Check the connection
if ($conn->connect_error) {
    die("Failed to connect: " . $conn->connect_error);
} else {
    echo "Connected";
}

// Close the connection (not mandatory as PHP automatically closes it at the end of the script)
$conn->close();
?>
```

Replace the placeholder variables with the actual values and run the code:

```console
[student@ip-172-31-88-88 ~]$ vi test_conn.php
[student@ip-172-31-88-88 ~]$ php test_conn.php 
Connected[student@ip-172-31-88-88 ~]$
```


## Lab

1. Install php, php-mysqlnd and php-cli on your system. 

2. Ensure that httpd and mariadb is installed, else install them.

3. Start the httpd service, or restart the httpd service if it was already running. 

4. Edit the file `phpinfo.php` and access the webpage from the web.  

5. Modify the code in this [section](#connecting-to-mariadb-using-php) to connect to your database server.  Ensure that the connection is successful.  


## Conclusion

In this lab we have installed PHP on a webserver and investigated running PHP code.  We have also wrote PHP code to connect to a MariaDB database.  


## References

1. [Introduction to PHP](https://php.org/introduction-to-php/)

2. [phpinfo](https://www.php.net/manual/en/function.phpinfo.php)