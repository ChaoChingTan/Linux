# Objectives
- [Root users vs normal users](#root-users-vs-normal-users)
- [**id** command](#id-command)
- [Elevating to root](#elevating-to-root)
- [Creating and deleting users](#creating-and-deleting-users)
- [Important files](#important-files)
    - /etc/passwd
    - /etc/shadow
    - /etc/group
- [Managing Groups](#managing-groups)
    - [Creating groups](#creating-groups)
    - [Adding Users to a Group with useradd](#adding-user-to-a-group-with-useradd)
    - [Modifying User's Group with usermod](#modifying-users-group-with-usermod)

## Introduction
Linux is a multi user operating system.  There can be multiple users logged into the system at any point in time.  

2 types of users exist on Linux systems.  Normal users and the root user.  

Normal users are standard user accounts created for regular system users. These accounts have limited privileges and can only access or modify files and settings for which they have permission. Normal users cannot perform critical system-level tasks, such as modifying system files or installing software globally, without elevated privileges.  

Root users on the other hand, can do absolutely anything and everything on a Linux system, including potentially destructive, non-reversible commands.  

## Root users vs normal users

A user typically uses a non privileged account to log into a Linux system as a normal user.  You can use the **id** command to check the details of the current login user.  

## **id** command

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


## Managing Groups
When you create a user on a Linux system, a private group with the same name as the username is created automatically and assigned to that user.

```bash
[root@localhost]# useradd student
[root@localhost]# id student
uid=1001(student) gid=1001(student) groups=1001(student)
[root@localhost]# 
```
Every user has a primary group.  This is indicated by the `gid` of the user.  In the example above, the user `student` has a primary group id of `1001` corresponding to the group name `student`.

### Creating Groups

To create a group in Linux, user the command `groupadd`.  Note that this command will fail if it is not executed as root:

```bash
[student@localhost]$ groupadd sales
groupadd: Permission denied.
groupadd: cannot lock /etc/group; try again later.
[student@localhost]$ 
```
Running the same command as the root user will be successful.  Note that this command adds an entry to the end of the `/etc/group` file, as illustrated below:

```bash
[root@localhost]# groupadd sales
[root@localhost]# tail -1 /etc/group
sales:x:1002:
```

The newly created group `sales` has a group id of `1002`.


### Adding User to a Group with useradd

Now that you created the new group, you can add users to the group.  For new users, this can be done during user creation when running the `useradd` command.  You can find out about the available options by running `useradd --help`:

```console
[root@localhost]# useradd --help
Usage: useradd [options] LOGIN
       useradd -D
       useradd -D [options]

Options:

<Output truncated>
  -g, --gid GROUP               name or ID of the primary group of the new account
  -G, --groups GROUPS           list of supplementary groups of the new account
```

Let's create a user `student2` and assigning it a primary group of `sales`:

```bash
[root@localhost]# useradd -g sales student2
[root@localhost]# id student2
uid=1002(student2) gid=1002(sales) groups=1002(sales)
[root@localhost]# 
```
Note the group id assigned to the student is `1002` with the corresponding group name `sales`

### Modifying User's Group with usermod
For exising user, you can modify the user's group with the `usermod` command.  You can investigate the various options by running `usermod --help`:

```bash
[root@localhost]# usermod --help
Usage: usermod [options] LOGIN

Options:
  -b, --badnames                allow bad names
  -c, --comment COMMENT         new value of the GECOS field
  -d, --home HOME_DIR           new home directory for the user account
  -e, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -f, --inactive INACTIVE       set password inactive after expiration to INACTIVE
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -a, --append                  append the user to the supplemental GROUPS mentioned by the -G option without removing
                                the user from other groups
  -h, --help                    display this help message and exit
  -l, --login NEW_LOGIN         new value of the login name
  -L, --lock                    lock the user account
  -m, --move-home               move contents of the home directory to the new location (use only with -d)
  -o, --non-unique              allow using duplicate (non-unique) UID
  -p, --password PASSWORD       use encrypted password for the new password
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             new login shell for the user account
  -u, --uid UID                 new UID for the user account
  -U, --unlock                  unlock the user account
  -v, --add-subuids FIRST-LAST  add range of subordinate uids
  -V, --del-subuids FIRST-LAST  remove range of subordinate uids
  -w, --add-subgids FIRST-LAST  add range of subordinate gids
  -W, --del-subgids FIRST-LAST  remove range of subordinate gids
  -Z, --selinux-user SEUSER     new SELinux user mapping for the user account

```

Each user can have only **one** primary group.  To set a user's primary group, user the `-g` option:

```bash
[root@localhost]# id student2
uid=1002(student2) gid=1002(sales) groups=1002(sales)
[root@localhost]# usermod -g adm student2
[root@localhost]# id student2
uid=1002(student2) gid=4(adm) groups=4(adm)
```

Each user can aditionally belong to **one or more** supplementary groups.  To **set** a user's supplementary group, use the `-G` option:

```bash
[root@localhost]# usermod -G sales student2
[root@localhost]# id student2
uid=1002(student2) gid=4(adm) groups=4(adm),1002(sales)
```

To **append** to the existing supplementary group of the user, use the `-a` option:

```bash
[root@localhost]# id student2
uid=1002(student2) gid=4(adm) groups=4(adm),1002(sales)
[root@localhost]# usermod -G wheel student2
[root@localhost]# id student2
uid=1002(student2) gid=4(adm) groups=4(adm),10(wheel)
[root@localhost]# usermod -aG adm student2
[root@localhost]# id student2
uid=1002(student2) gid=4(adm) groups=4(adm),10(wheel)
```

Note that without the `-a` option in the example above, the supplementary group gets overwritten.  

## Lab
1. Create the group *eve* with the gid 2000.  You may want to check the /etc/group file to ensure that the group is created.
2. Create a user with username *eve* with the following specifications (in one command):
    - numerical uid should be 2000
    - numerical group id should be 2000
    - login shell should be /bin/sh
3. Check the userid and group id of user *eve* using the **id** command
4. Check the entry in /etc/passwd file and find the userid and groupid of user *eve*
5. Create a group with group name *evil*
6. Change the primary group of user *eve* to *evil*, verify this using the **id** command
7. Add the group *eve* and *wheel* to the supplementary group of user *eve*
8. Delete the user *eve* from the system
9. Delete the group *evil* from the systems 


## Conclusion
In this lab we have investigated the differences between a normal and a root user, and how to switch between them.  

We have also learnt how to create and delete users on the system.  We have also experimented creating groups and changing user primary and supplementary groups.  

## References
[Linux Fundamentals, Paul Cobbaut -  Chapter 27-29, 31](https://linux-training.be/linuxfun.pdf)
