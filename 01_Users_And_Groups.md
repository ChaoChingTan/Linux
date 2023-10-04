# Objectives
- Root users vs normal users
- **id** command
- **sudo** command
- Creating and deleting users
- Important files
    - /etc/passwd
    - /etc/shadow
    - /etc/group

## Introduction
Linux is a multi user operating system.  There can be multiple users logged into the system at any point in time.  

2 types of users exist on Linux systems.  Normal users and the root user.  

Normal users are standard user accounts created for regular system users. These accounts have limited privileges and can only access or modify files and settings for which they have permission. Normal users cannot perform critical system-level tasks, such as modifying system files or installing software globally, without elevated privileges.  

Root users on the other hand, can do absolutely anything and everything on a Linux system, including potentially destructive, non-reversible commands.  

## Root users vs normal users

A user typically uses a non privileged account to log into a Linux system as a normal user.  You can use the **id** command to check the details of the current login user.  

```bash
[student@ip-172-31-88-88 ~]$id
uid=1001(student) gid=1001(student) groups=1001(student) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[student@ip-172-31-88-88 ~]$
```

Notice that the command prompt ends with a '$'.  This indicate that the logged in user is not a root user.  The command prompt is typically '#' for root.  

The output shows the logged in userid *uid*, the user's groups and group ids *gid*.  The *context* fields shows the SELinux security context of the user.

## Elevating to root
A user can elevate to root by using either of the following commands:
- **su**
- **sudo**

**su** allows commands to be run with a substitute user and group ID.  The root password is required in order to run this command:

```bash
[student@ip-172-31-88-88 ~]$su
Password: 
[root@ip-172-31-92-134 student]# 
```
Note that the prompt changes to '#' for the root user.  

Type **exit** to return to normal user. 

```bash
[root@ip-172-31-92-134 student]# exit
exit
[student@ip-172-31-88-88 ~]$
```

On many production Linux systems, it is typical that users do not know the root password.  Users can be configured to run specific commands using the **sudo** command without knowing the root password.  

```bash
[student@ip-172-31-88-88 ~]$sudo bash
[root@ip-172-31-92-134 student]# 
```
In the example above, we execute the command **sudo bash** to run the bash shell as the root user.  You can further restrict the commands that can be run by configuring the */etc/sudoers* file.  

Type **exit** to return to normal user. 

```bash
[root@ip-172-31-92-134 student]# exit
exit
[student@ip-172-31-88-88 ~]$
```


## Creating and deleting users
Only root can create / delete users.  To create users, use the **useradd** command:

```bash
[student@ip-172-31-88-88 ~]$useradd user1
useradd: Permission denied.
useradd: cannot lock /etc/passwd; try again later.
[student@ip-172-31-88-88 ~]$sudo useradd user1
[student@ip-172-31-88-88 ~]$
```
Notice in the example that you will need to run the command as root (using **sudo**) for it to be successful.  

To delete a user, use the userdel command:
```bash
[student@ip-172-31-88-88 ~]$sudo userdel user1
[student@ip-172-31-88-88 ~]$
```

## Important files
The */etc/passwd* file stores user login information such as username, userid, primary group id, home directory and login shell:

```bash
[student@ip-172-31-88-88 ~]$grep student /etc/passwd
student:x:1001:1001::/home/student:/bin/bash
[student@ip-172-31-88-88 ~]$
```
The */etc/shadow* file stores the hashed password of users on the system.  It is not readable by normal users.  

The */etc/group* file stores information about user groups on the system. 


## Lab
1. Create a user with username *eve* 
2. Check the userid of user *eve* using the **id** command
3. Check the entry in /etc/passwd file and find the userid and groupid of user *eve*
4. Delete the user *eve* from the system

## Conclusion
In this lab we have investigated the differences between a normal and a root user, and how to switch between them.  

We have also learnt how to create and delete users on the system.  

## References
[Linux Fundamentals, Paul Cobbaut -  Chapter 27-29, 31](https://linux-training.be/linuxfun.pdf)
