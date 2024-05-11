Install PHP on Apache Web Server in Ubuntu 

The PHP programming language is widely used for server-side web applications. Such as, Wordpress, OpenEMR, Joomla, phpBB, Magento and others. 

Enter the following commands in the terminal: 

$ sudo apt install php libapache2-mod-php   

Install essential PHP modules 

Enter the following commands in the terminal: 

$ sudo apt install php-cli  
$ sudo apt install php-cgi  
$ sudo apt install php-mysql 
$ sudo systemctl restart apache2  

Test PHP installation 

Add a web page to the Apache default web site.  

Enter the following commands in the terminal: 

$ sudo nano /var/www/html/phpinfo.php  

In the nano editor enter the following code: 

<?php phpinfo(); ?>  

 Press Control + X, then press Y to save. 

Open your browser window and enter the following URL: 

[http://localhost/index.php](http://localhost/index.php)  

If successful, you will see a page that shows information about your PHP installation 

Install PHP modules required for OpenEMR 

The following modules must be installed in order for OpenEMR to work. 

Enter the following commands in the terminal: 

$ sudo apt install php-curl  $ sudo apt install php-gd  $ sudo apt install php-ldap  $ sudo apt install php-mbstring  $ sudo apt install php-soap  $ sudo apt install php-xml  $ sudo apt install php-zip  

Install mCrypt 

OpenEMR requires the use of mcrypt which is no longer installed in PHP by default. This means you have to install this manually. 

Enter the following commands in the terminal: 

# install php dev $ sudo apt install php-dev   # install build tools $ sudo apt -y install gcc make autoconf libc-dev pkg-config   # install mcrypt dev $ sudo apt -y install libmcrypt-dev   # build and install mcrypt $ sudo pecl install mcrypt-1.0.3   # configure php to use mcrypt $sudo bash -c "echo extension=/usr/lib/php/20190902/mcrypt.so > /etc/php/7.4/cli/conf.d/mcrypt.ini"  $sudo bash -c "echo extension=/usr/lib/php/20190902/mcrypt.so > /etc/php/7.4/apache2/conf.d/mcrypt.ini"   # test mcrypt $ sudo php -i | grep "mcrypt"  

OpenEMR Architecture 

This article shows all of the PHP and other dependencies that OpenEMR needs to operate properly. The installation process of OpenEMR assumes that you have already installed all of these modules. If you followed the runbooks in this section then you have but this is good reference. 

[https://www.open-emr.org/wiki/index.php/OpenEMR_System_Architecture#OpenEMR_Dependencies](https://www.open-emr.org/wiki/index.php/OpenEMR_System_Architecture#OpenEMR_Dependencies) 

To see the installed PHP modules enter the following command in terminal: 

$ sudo php -m