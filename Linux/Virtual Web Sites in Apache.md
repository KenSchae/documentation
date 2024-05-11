## Introduction 

Don't drop virtual sites into /var/www/html. You could end up with a situation where your virtual site is accessible via the default web site AND your virtual definition. It's just better to put your virtual sites into their own folders.
## Virtual Web Sites 

Virtual Web Sites are the mechanism that allows you to host more than one domain on a single Apache web server. For example, using virtual sites I can host [http://www.kenschae.com](http://www.kenschae.com/) in one folder and [http://www.mygreatsite.com](http://www.mygreatsite.com/) in another folder. This logic applies to hosting multiple hosts under a single domain. For example, I can host [http://intranet.kenschae.com](http://intranet.kenschae.com/) in one folder and [http://blog.kenschae.com](http://blog.kenschae.com/) in another. Finally, you can use the same approach to host unregistered domains that are only visible on your local network (if you have a DNS server). For example, I can host [http://dev.internal.com](http://dev.internal.com/) in one folder and [http://intranet.internal.com](http://intranet.internal.com/) in another and [http://blog.fakedomain.com](http://blog.fakedomain.com/) in yet another. 

For consistency and maintenance, you should have some kind of naming scheme or tracker to keep it all straight. 

## Setup a virtual web site 

The example below will create a new virtual web site at site.local when creating your own virtual site replace the references for site.local to your host.domain combination. 

Enter the following commands in the terminal: 
```
# create the folder to host the virtual site 
$ cd /var/www 
$ sudo mkdir /var/www/site.local  

# give Apache access to the files and folders in the virtual site 
$ sudo chgrp -R www-data /var/www/site.local 
$ sudo find /var/www/site.local -type d -exec chmod g+rx {​​}​​​​​​​​​ + 
$ sudo find /var/www/site.local -type f -exec chmod g+r {​​​​​​​​​}​​​​​​​​​ +  

# give your owner read/write permissions to the files and folders 
# replace the word USERNAME with your user account name. 
$ sudo chown -R USERNAME /var/www/site.local 
$ sudo find /var/www/site.local -type d -exec chmod u+rwx {​​​​​​​​​}​​​​​​​​​ + 
$ sudo find /var/www/site.local -type f -exec chmod u+rw {​​​​​​​​​}​​​​​​​​​ +  

# all new files will be created with www-data as the access user [optional] 
$ sudo find /var/www/site.local -type d -exec chmod g+s {​​​​​​​​​}​​​​​​​​​ +  

# prevent other users from seeing the data in the files and folders [optional] 
$ sudo chmod -R o-rwx /var/www/site.local  

# create a test web page for the site $ cd /var/www/site.local/public_html 
$ sudo nano index.html  
```


In the nano editor add the following code 

`<html> <head><title>Site</title></head><body><h1>Site</h1></body> </html>`  

Press Ctrl+X then Y to save the web page. 

Configure Apache to use the new site 

Enter the following commands in the terminal: 

$ cd /etc/apache2/sites-available  $ sudo cp 000-default.config site.local.conf  $ sudo nano site.local.conf  

In the nano editor modify the config file to: 

<VirtualHost *:80>     # The ServerName directive sets the request scheme, hostname and port that     # the server uses to identify itself. This is used when creating     # redirection URLs. In the context of virtual hosts, the ServerName     # specifies what hostname must appear in the request's Host: header to     # match this virtual host. For the default virtual host (this file) this     # value is not decisive as it is used as a last resort host regardless.     # However, you must set it for any further virtual host explicitly.     ServerName [s](https://www.example.com/)ite.local     ServerAlias [www.site.local](http://www.site.local/)      ServerAdmin [webmaster@site.local](mailto:webmaster@site.local)     DocumentRoot /var/www/site.local      # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,     # error, crit, alert, emerg.     # It is also possible to configure the loglevel for particular     # modules, e.g.     #LogLevel info ssl:warn      ErrorLog ${​​​​​​APACHE_LOG_DIR}​​​​​​/site.local-error.log     CustomLog ${​​​​​​APACHE_LOG_DIR}​​​​​​/site.local-access.log combined      # For most configuration files from conf-available/, which are     # enabled or disabled at a global level, it is possible to     # include a line for only one particular virtual host. For example the     # following line enables the CGI configuration for this host only     # after it has been globally disabled with "a2disconf".     #Include conf-available/serve-cgi-bin.conf </VirtualHost>  

Press Ctrl+X then Y to save the config file. 

Enter the following commands in the terminal: 

$ sudo a2ensite site.local  $ sudo apachectl configtest  $ sudo systemctl restart apache2