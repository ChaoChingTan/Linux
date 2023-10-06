# The Apache HTTP Server

## Objectives

- Installing the Apache webserver
- httpd.conf
- access_log
- error_log
- index.html

## Installing the Apache webserver

To install the Apache webserver, the `dnf install` command can be used as with any software installation in Linux.  Refer to the [lab](https://github.com/ChaoChingTan/labs/blob/main/Linux_Software_Installation.md) on software installation specifically the [example](https://github.com/ChaoChingTan/labs/blob/main/Linux_Software_Installation.md#example---software-installation--removal) on how to install the `httpd` package.  

## httpd.conf

The main configuration file for httpd is `httpd.conf` and is found commonly under the directory `/etc/httpd/conf`:

```console
[student@ip-172-31-88-88 ~]$ ls -l /etc/httpd/conf
total 28
-rw-r--r--. 1 root root 12005 Apr 28 16:39 httpd.conf
-rw-r--r--. 1 root root 13430 Apr 28 16:41 magic
[student@ip-172-31-88-88 ~]$ 
```
The Apache webserver is highly customisable to suit different needs.  The configuration is done via [directives](https://httpd.apache.org/docs/2.4/mod/directives.html).  Some examples include:

- *Listen* which specifies the HTTP listening port or specific IP address
- *DocumentRoot* which sets the directory from which httpd will serve files
- *User* The userid under which the server will answer requests
- *Group* Group under which the server will answer requests

Default settings are set in the `httpd.conf`, and these can be overwritten.  

```console
[student@ip-172-31-88-88 ~]$ egrep "^Listen|^DocumentRoot|^User|^Group" /etc/httpd/conf/httpd.conf 
Listen 80
User apache
Group apache
DocumentRoot "/var/www/html"
[student@ip-172-31-88-88 ~]$ 
```

The http server in the example shown above is listening on all interfaces on port 80.  The process is running as user apache and group apache.  Files are served out of the directory `/var/www/html`.  


## access_log

Access logs for httpd can typically be found at `/var/log/httpd/access_log`.  Access logs show the requests to the web server.  It is common to see potentially malicious scans looking for vulnerabilities on the server.  

```console
[student@ip-172-31-88-88 ~]$ sudo tail -1 /var/log/httpd/access_log
XX.XX.XX.XX - - [05/Oct/2023:05:52:57 +0000] "GET /phpMyAdmin_/index.php?lang=en HTTP/1.1" 404 196 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36"
[student@ip-172-31-88-88 ~]$ 
```

## error_log

Error logs for httpd can typically be found at `/var/log/httpd/error_log`.  

```console
[student@ip-172-31-88-88 ~]$ sudo tail -1 /var/log/httpd/error_log
[Thu Oct 05 06:36:38.901546 2023] [autoindex:error] [pid 659:tid 836] [client XX.XX.XX.XX:XXXXXX] AH01276: Cannot serve directory /var/www/html/: No matching DirectoryIndex (index.html) found, and server-generated directory index forbidden by Options directive
```

Investigate the access_log and error_log in case of issues with httpd.  

## index.html

The *DirectoryIndex* directive in the `httpd.conf` specifies the default file to serve when a user accesses a directory on a web server (e.g., http://127.0.0.1/), without specifying a specific file.  *DirectoryIndex* defaults to index.html in most installations:

```console
[student@ip-172-31-88-88 ~]$ grep DirectoryIndex /etc/httpd/conf/httpd.conf 
# DirectoryIndex: sets the file that Apache will serve if a directory
    DirectoryIndex index.html
```
This means that the content of this file will be served to users when the access the web server without specifying a specific file.  


## Lab

1. Install the httpd package.  Refer to the [chapter on software installation](https://github.com/ChaoChingTan/labs/blob/main/Linux_Software_Installation.md) for procedures.

2. Start **and enable** the httpd service.  Refer to the [chapter on services](https://github.com/ChaoChingTan/labs/blob/main/Linux_Services.md) for procedures.  

3. Modify any existing firewall rules to allow access to the HTTP port, and verify that the http server can be access via a web browser.

4. Create a file `/var/www/html/index.html` with the content `Hello World` and access it from the web browser.  Refer to the [chapter on editing files](03_Editing_Files.md) for procedures. Note that the directory `/var/www/html` is only writable by root so you will have to have root privileges. 

5. Create a file `/var/www/html/mypage.html` with the content `I am foo-bar and having fun with Linux` and access it from the web browser.  Replace foo-bar with foo-ever you are.  Note that the directory `/var/www/html` is only writable by root so you will have to have root privileges. 


## Conclusion

In this chapter we have installed a webserver, started and enabled the HTTP server.  We have investigated the important files of the Apache web server:  httpd.conf, access_log, error_log.

We have also created some files that we can access from the webserver:  index.html and mypage.html


## References

1. [Apache HTTP Server Project](https://projects.apache.org/project.html?httpd-http_server)