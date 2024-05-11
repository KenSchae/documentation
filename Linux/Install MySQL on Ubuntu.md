## Install MySQL on Ubuntu 

```
$ sudo apt install mysql-server   
```
## Enable MySQL on Ubuntu 

This step will add MySQL to the system startup so that MySQL will start every time that the computer starts. 

```
$ sudo systemctl enable mysql
```
## Secure MySQL (For Production) 

It is imperative to "button up" your installation of MySQL. This prevents hackers from getting into your databases. 

```
$ sudo mysql_secure_installation  
$ Press y|Y for Yes, any other key for No: y  
$ Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1  
$ Remove anonymous users? Y  
$ Disallow root login remotely? Y  
$ Remove test database? Y  
```

## Log in to MySQL 

```
$sudo mysql -u root -p  
```

Create a new MySQL user because using root is BAD! 

```
CREATE USER 'sqladmin'@'localhost' IDENTIFIED BY 'password'; 
GRANT ALL PRIVILEGES ON *.* TO 'sqladmin'@'localhost' WITH GRANT OPTION; 
FLUSH PRIVILEGES; 
exit;   
```
## Install MySQL Workbench on Ubuntu Desktop 

Enter the following commands in the terminal: 

$ sudo apt install mysql-workbench  

Enable local_infile for OpenEMR 

OpenEMR requires the use of local files for uploading content to database tables 

Enter the following commands in the terminal: 

$ sudo mysql -u root  mysql> SET GLOBAL local_infile = 'ON';  mysql> SHOW GLOBAL VARIABLES LIKE 'local_infile';  

Allow Remote Connections 

[https://linuxize.com/post/mysql-remote-access/](https://linuxize.com/post/mysql-remote-access/) 

[How to Allow Remote MySQL Connections (phoenixnap.com)](https://phoenixnap.com/kb/mysql-remote-connection) 

References 

[https://www.rosehosting.com/blog/how-to-install-mysql-on-ubuntu-18-04/](https://www.rosehosting.com/blog/how-to-install-mysql-on-ubuntu-18-04/) 

[https://phoensudo mysql -u root -pixnap.com/kb/install-get-started-mysql-workbench-on-ubuntu](https://phoenixnap.com/kb/install-get-started-mysql-workbench-on-ubuntu) 

[https://mi-squared.com/2019/11/05/openemr-external-data-load-query-error/](https://mi-squared.com/2019/11/05/openemr-external-data-load-query-error/)