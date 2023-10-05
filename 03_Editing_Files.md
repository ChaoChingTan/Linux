# Editing Files

## Objectives

- **gedit**
- **vi** and **vim**
- **nano**

## Introduction

For a new Linux user using GNOME desktop environment, the command `gedit` serves the purpose of editing files well.

In an environment without a GUI interface, the command `vi` can be used to view and edit files. The command `vimtutor` which is installed as part of the `vim` package provides a good tutorial to those getting started with `vi`. 


## **gedit**

In an environment using GNOME desktop environment, you can run the `gedit` command and it works quite similar to the Windows Notepad application that we are familiar with: ![gedit interface](assets/03_01_gedit.png)


## **vi** and **vim**

`vi` is the default editor installed on most Linux distributions. One of the easiest way to learn about how to use `vi` is via `vimtutor`. 

`vimtutor` steps you through vi with step by step tutorials.  It my not be installed by default.  


```bash
[student@ip-172-31-88-88 ~]$ vimtutor
-bash: vimtutor: command not found
[student@ip-172-31-88-88 ~]$ 
```

`vimtutor` is available in the `vim` package.  Install this package if it is not yet installed on your system.  

```bash
[student@ip-172-31-88-88 ~]$ sudo dnf install vim -y
Updating Subscription Management repositories.
Unable to read consumer identity

This system is not registered with an entitlement server. You can use subscription-manager to register.

Last metadata expiration check: 2:15:39 ago on Thu 05 Oct 2023 05:48:08 AM UTC.
Dependencies resolved.
================================================================================
 Package         Arch    Version              Repository                   Size
================================================================================
Installing:
 vim-enhanced    x86_64  2:8.2.2637-20.el9_1  rhel-9-appstream-rhui-rpms  1.8 M
Installing dependencies:
 gpm-libs        x86_64  1.20.7-29.el9        rhel-9-appstream-rhui-rpms   22 k
 vim-common      x86_64  2:8.2.2637-20.el9_1  rhel-9-appstream-rhui-rpms  7.0 M
 vim-filesystem  noarch  2:8.2.2637-20.el9_1  rhel-9-baseos-rhui-rpms      22 k

Transaction Summary
================================================================================
Install  4 Packages

Total download size: 8.8 M
Installed size: 34 M
Downloading Packages:
(1/4): gpm-libs-1.20.7-29.el9.x86_64.rpm        329 kB/s |  22 kB     00:00    
(2/4): vim-filesystem-8.2.2637-20.el9_1.noarch. 1.4 MB/s |  22 kB     00:00    
(3/4): vim-enhanced-8.2.2637-20.el9_1.x86_64.rp  16 MB/s | 1.8 MB     00:00    
(4/4): vim-common-8.2.2637-20.el9_1.x86_64.rpm   38 MB/s | 7.0 MB     00:00    
--------------------------------------------------------------------------------
Total                                            34 MB/s | 8.8 MB     00:00     
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Installing       : vim-filesystem-2:8.2.2637-20.el9_1.noarch              1/4 
  Installing       : vim-common-2:8.2.2637-20.el9_1.x86_64                  2/4 
  Installing       : gpm-libs-1.20.7-29.el9.x86_64                          3/4 
  Installing       : vim-enhanced-2:8.2.2637-20.el9_1.x86_64                4/4 
  Running scriptlet: vim-enhanced-2:8.2.2637-20.el9_1.x86_64                4/4 
  Verifying        : gpm-libs-1.20.7-29.el9.x86_64                          1/4 
  Verifying        : vim-common-2:8.2.2637-20.el9_1.x86_64                  2/4 
  Verifying        : vim-enhanced-2:8.2.2637-20.el9_1.x86_64                3/4 
  Verifying        : vim-filesystem-2:8.2.2637-20.el9_1.noarch              4/4 
Installed products updated.

Installed:
  gpm-libs-1.20.7-29.el9.x86_64                                                 
  vim-common-2:8.2.2637-20.el9_1.x86_64                                         
  vim-enhanced-2:8.2.2637-20.el9_1.x86_64                                       
  vim-filesystem-2:8.2.2637-20.el9_1.noarch                                     

Complete!
[student@ip-172-31-88-88 ~]$
```
Run the `vimtutor` command to start the Vim Tutor:

```bash
===============================================================================
=    W e l c o m e   t o   t h e   V I M   T u t o r    -    Version 1.7      =
===============================================================================

     Vim is a very powerful editor that has many commands, too many to
     explain in a tutor such as this.  This tutor is designed to describe
     enough of the commands that you will be able to easily use Vim as
     an all-purpose editor.

     The approximate time required to complete the tutor is 30 minutes,
     depending upon how much time is spent with experimentation.

     ATTENTION:
     The commands in the lessons will modify the text.  Make a copy of this
     file to practice on (if you started "vimtutor" this is already a copy).

     It is important to remember that this tutor is set up to teach by
     use.  That means that you need to execute the commands to learn them
     properly.  If you only read the text, you will forget the commands!

     Now, make sure that your Caps-Lock key is NOT depressed and press
     the   j   key enough times to move the cursor so that lesson 1.1
     completely fills the screen.
"/tmp/tutorNKy5Qo" 972 lines, 33435 bytes
```

Run through the tutorial to practice **vi**.  To exit vimtutor, press ESC followed by ':q!' followed by the return key.


## **nano**

For beginners, some may prefer `nano`.  Install `nano` and run the command to bring up the following interface: 

```bash
  GNU nano 5.6.1                      New Buffer                                




















               [ Welcome to nano.  For basic help, type Ctrl+G. ]
^G Help      ^O Write Out ^W Where Is  ^K Cut       ^T Execute   ^C Location
^X Exit      ^R Read File ^\ Replace   ^U Paste     ^J Justify   ^_ Go To Line
```

After text input, when the user tries to quit by press Ctrl-X, `nano` will prompt the user to save the file:

```bash
  GNU nano 5.6.1                      New Buffer                      Modified  
The quick brown	fox jumps over the lazy	dog



















Save modified buffer?                                                           
 Y Yes
 N No           ^C Cancel
```

User can then specify the filename to save to.

```bash
  GNU nano 5.6.1                      New Buffer                      Modified  
The quick brown	fox jumps over the lazy	dog



















File Name to Write: myfile                                                      
^G Help             M-D DOS Format	M-A Append          M-B Backup File
^C Cancel           M-M Mac Format	M-P Prepend         ^T Browse
```

The file will be saved according to the filename you specified (*myfile* in the example above)

```bash
[student@ip-172-31-88-88 ~]$ ls -la
total 28
drwx------. 4 student student  175 Oct  5 08:23 .
drwxr-xr-x. 5 root    root      50 Oct  4 04:54 ..
-rw-------. 1 student student 1018 Oct  5 07:54 .bash_history
-rw-r--r--. 1 student student   18 Nov 24  2022 .bash_logout
-rw-r--r--. 1 student student  141 Nov 24  2022 .bash_profile
-rw-r--r--. 1 student student  492 Nov 24  2022 .bashrc
drwxr-xr-x. 3 student student   18 Oct  4 07:55 .cache
drwx------. 3 student student   28 Oct  5 08:13 .emacs.d
-rw-------. 1 student student   32 Oct  5 08:05 .lesshst
-rw-r--r--. 1 student student   44 Oct  5 08:23 myfile
-rwxrw----. 1 student student    0 Oct  4 09:57 testfile
-rw-------. 1 student student  733 Oct  5 08:09 .viminfo
[student@ip-172-31-88-88 ~]$ 
```

You can view the file content to verify:

```bash
[student@ip-172-31-88-88 ~]$ cat myfile 
The quick brown fox jumps over the lazy dog
[student@ip-172-31-88-88 ~]$
```

## Lab

1. Edit a file called `hello.txt` containing the string `Linux rocks!` 

2. You can use any editor of your choice.  If the editor you want to use is not installed, go ahead and install it.

3. Verify the content of the file `hello.txt`


## Conclusion

In this lab we have introduced the `vi`, `vim` and `nano` editors.  For learning `vi`, you can use `vimtutor` which is part of the `vim` package. 


## References

1. [Linux Fundamentals, Paul Cobbaut -  Chapter 22: Introduction to vi](https://linux-training.be/linuxfun.pdf)