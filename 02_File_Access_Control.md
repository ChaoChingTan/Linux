# Objectives

- Basic permissions - read, write, execute
- **chmod** 
- Ownership, **chown** and **chgrp** 
- Special permissions - setuid, setgid, sticky

## Introduction
Permissions and ownership in Linux are crucial for security and access control, ensuring that users have appropriate rights to read, write, and execute files, and preventing unauthorized access or modifications.


## Basic permissions - read, write, execute

Let's begin by investigating the files in the home directory.  We do this by listing the directory content using `ls -la` command.  `-l` produces a long listing and `-a` shows hidden files (files that start with '.')

```bash
[student@ip-172-31-88-88 ~]$ ls -la
total 20
drwx------. 3 student student 129 Oct  4 10:00 .
drwxr-xr-x. 5 root    root     50 Oct  4 04:54 ..
-rw-------. 1 student student 463 Oct  4 08:03 .bash_history
-rw-r--r--. 1 student student  18 Nov 24  2022 .bash_logout
-rw-r--r--. 1 student student 141 Nov 24  2022 .bash_profile
-rw-r--r--. 1 student student 492 Nov 24  2022 .bashrc
drwxr-xr-x. 3 student student  18 Oct  4 07:55 .cache
-rw-------. 1 student student  32 Oct  4 09:24 .lesshst
-rw-r--r--. 1 student student   0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$
```

Let's take a closer look at the permissions of the file *testfile*:

```bash
-rw-r--r--. 1 student student   0 Oct  4 09:57 testfile
```

The first character indicates the type of file. In this case, it's a regular file (-). Other variants include:

- `b` for block devices
- `c` for character devices
- `d` for directories
- `l` for symbolic links

The next nine characters represent the permissions. They are divided into three sets of three characters each for the owner, group, and others:

- rw- (read and write, no execute) for the owner, in this case the owner is *student*
- r-- (read, no write, no execute) for the group, in this case the group is *student*
- r-- (read, no write, no execute) for others, anyone who is not owner or group owner.

### Read (r):
- For Files: Allows the user to view the contents of the file.
- For Directories: Allows the user to list the names of files in the directory.

### Write (w):
- For Files: Allows the user to modify the content of the file, including creating, deleting, or modifying data.
- For Directories: Allows the user to create, delete, or rename files within the directory.

### Execute (x):
- For Files: Allows the user to execute the file if it is a program or script.
- For Directories: Allows the user to access the directory, meaning the user can navigate into it.


## **chmod** 

The `chmod` command is used to change the permissions of files or directories.  The following example adds execute permission for the owner (u+x), gives the group read and write permissions (g=rw), and removes all permissions for others (o-rwx): 

```bash
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rw-r--r--. 1 student student 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ chmod u+x,g=rw,o-rwx testfile 
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 student student 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ 
```

## Ownership, **chown** and **chgrp**

Using the same example, the file testfile is owned by the user *student* and the group of the same name *student*.  

To change the ownership, use the `chown` command.  Note that root privilege is required:

```bash
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 student student 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ chown root testfile 
chown: changing ownership of 'testfile': Operation not permitted
[student@ip-172-31-88-88 ~]$ sudo chown root testfile 
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 root student 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$
```

To change the group ownership, use the `chgrp` command.  Note that root privilege is required:

```bash
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 root student 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ chgrp root testfile 
chgrp: changing group of 'testfile': Operation not permitted
[student@ip-172-31-88-88 ~]$ sudo chgrp root testfile 
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 root root 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ 
```

The `chown` command can actually be used to change the group ownership.  Refer to the example below and the man page for details:

```bash
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 root root 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ sudo chown student:student testfile 
[student@ip-172-31-88-88 ~]$ ls -l testfile 
-rwxrw----. 1 student student 0 Oct  4 09:57 testfile
[student@ip-172-31-88-88 ~]$ 
```

## Special permissions - setuid, setgid, sticky

Besides the basic permissions discussed, there are also special permissions - setuid, setgid and sticky.  

- setuid (Set User ID) (s):
    - When the setuid bit is set on an executable file, the program will run with the privileges of the file's owner, not the user who executes it.

- setgid (Set Group ID) (s):
    - When the setgid bit is set on an executable file, the program will run with the privileges of the file's group, not the user's primary group.
    - On directories, when the setgid bit is set, newly created files within the directory inherit the group ownership of the parent directory.

- sticky (t):
    - When the sticky bit is set on a directory, only the file owner can delete or rename their files within that directory, even if others have write permissions.
    - Often applied to directories like /tmp to ensure that users can only remove their own files.

These special permission bits are represented by the characters "s" (setuid or setgid) or "t" (sticky) in the output of ls -l. For example:

- `rwsr-xr-x` indicates setuid is set.
- `rwxr-sr-x` indicates setgid is set.
- `drwxrwxrwt` indicates the sticky bit is set on a directory.


## Conclusion
In this chapter, we have investigated basic read, write, execute permissions, and used the **chmod** command to change the permissions. 

We have also investigated file ownerships, and used the  **chown** and **chgrp** commands to manipulate ownerships.   

We have also touched on special file permissions: setuid, setgid and sticky.  


## References
1. [Linux Fundamentals, Paul Cobbaut - Chapter 32,33](https://linux-training.be/linuxfun.pdf)
2. ChatGPT. (2023, Oct 5)
3. [Linux SetUID](https://www.youtube.com/watch?v=lj35NETOuaM)
4. [SetUID vs no SetUID](https://www.youtube.com/watch?v=JP91JZRa3CY)
5. [Linux Sticky Bit](https://www.youtube.com/watch?v=F8y_W_tpsys) 