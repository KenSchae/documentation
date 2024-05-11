## Install Apache Web Server on Ubuntu 

```
$ sudo apt install apache2  
$ sudo systemctl enable apache2  
$ sudo systemctl start apache2  
```

Test the Apache Web Server installation 

From the server 

If you are logged into the server either directly or from an RDP connection, you can test the Apache installation following these steps. 

Open a web browser on the server and go to: [http://localhost](http://localhost/) 

You should see the Apache2 Ubuntu Default Page that says "It works" 


From a computer on the network 

If you want to test from another computer that is on the  same network as the Apache server, you will need some information. Specifically the IP address of the server.  

To get the IP address of the Apache server, login to the server and enter the following command in the terminal: 

$ sudo hostname -I  

That is a capital I and lower case i will tell you the local IP address which is almost always 127.0.0.1 

From the computer on the network, open a browser window and enter the IP address as the web page URL: [http://ipaddress](http://ipaddress/) For example on mine its [http://192.168.1.12](http://192.168.1.12/) 

You should see the same default It works page 

The Apache Default Web Site Directory 

The default web site in Apache on Ubuntu is at /var/www/html. If you try to edit or add to this site you will get permissions errors because the folders and files are owned by root. The temptation is to change the ownership of everything to your account and go on from there. While this may be acceptable for a test or dev server, it is a bad habit to get into. Rather use the following procedure to setup virtual web sites in your apache that you will then edit and leave the default web site alone.