# AWS RDS

## Objectives

- [Creating to a MariaDB RDS instance](#creating-to-a-mariadb-rds-instance)
- [Connecting to a MariaDB RDS instance](#connecting-to-a-mariadb-rds-instance)

## Introduction

While setting up and installing your own database server gives you more control, it also means more administrative overhead such as OS patching and database software upgrades.  Using a managed service such as AWS Relational Database Service (RDS) cuts down on these administrative tasks and use the database as a service.  Advance features such as high availability and scaling can be easily achieved.  

## Creating to a MariaDB RDS instance

Refer to the AWS [Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MariaDB.html), **Step 2** on how to create a MariaDB instance.  


## Connecting to a MariaDB RDS instance

To connect to the MaraDB on the RDS instance, use the command:

```console
mysql -u USER -h HOSTNAME_OR_IPADDR -p
```
The `USER` is the user which you have created during database [creation](#creating-to-a-mariadb-rds-instance).  Likewise the password to input is the one which you have set then.  

As for the `HOSTNAME_OR_IPADDR`, you can find the information from the AWS RDS dashboard.  Refer to the AWS [Guide](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MariaDB.html), **Step 3** on how to locate this information.  


## Lab

1. Install the maridb-server package if it is not already installed.

2. Create a MariaDB RDS instance by following the instructions [here](#creating-to-a-mariadb-rds-instance).  Note that in the step *Set up EC2 connection - optional* you should expand and fill up the information of the host that you will be connecting from.  Database creation takes a while to complete.  You may click on the refresh icon to see the latest status of the database creation.  

3. Connect to your MariaDB server using the mysql cli by following the instructions [here](#connecting-to-a-mariadb-rds-instance)

4. Show the databases in your MariaDB server

5. Exit the mysql client


## Conclusion

In this lab we have created a MariaDB database server running on AWS Relational Database Service (RDS).  We also use the mysql cli to connect to the database and interact with it.  

## References

1. [MariaDB](https://mariadb.org/)

2. [Creating and connecting to a MariaDB DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.MariaDB.html)