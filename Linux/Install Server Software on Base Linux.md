
## Traditional LAMP
In the old days, LAMP (Linux, Apache, MySQL, PHP) were the go-to web servers. These days, not as much. You will likely see more architectures centered around Docker or cloud rather than the old school LAMP environments.

### Install Apache Web Server

```
$ sudo apt install apache2  
$ sudo systemctl enable apache2  
$ sudo systemctl start apache2  
```

#### Test the Apache Web Server installation 

##### From the server 

If you are logged into the server either directly or from an RDP connection, you can test the Apache installation following these steps. 

Open a web browser on the server and go to: [http://localhost](http://localhost/) 

You should see the Apache2 Ubuntu Default Page that says "It works" 

##### From a computer on the network 

If you want to test from another computer that is on the  same network as the Apache server, you will need some information. Specifically the IP address of the server.  

To get the IP address of the Apache server, login to the server and enter the following command in the terminal: 

```
$ sudo hostname -I  

or 

$ ifconfig -a
```

That is a capital I and lower case i will tell you the local IP address which is almost always 127.0.0.1 

From the computer on the network, open a browser window and enter the IP address as the web page URL: [http://ipaddress](http://ipaddress/) For example on mine its [http://192.168.1.12](http://192.168.1.12/) 

You should see the same default It works page 

#### The Apache Default Web Site Directory 

The default web site in Apache on Ubuntu is at /var/www/html. If you try to edit or add to this site you will get permissions errors because the folders and files are owned by root. The temptation is to change the ownership of everything to your account and go on from there. While this may be acceptable for a test or dev server, it is a bad habit to get into. Rather use the following procedure to setup virtual web sites in your apache that you will then edit and leave the default web site alone.



## Other Servers

### Install Guest Agent for Proxmox
Applies only if this box is in a Proxmox server

1. Open Proxmox
2. Click Options
3. Turn on QEMU Agent

```
sudo apt install qemu-guest-agent
sudo shutdown 
```


### bind9 DNS
```
sudo apt install -y bind9 bind9utils bind9-doc dnsutils
sudo systemctl start named
sudo systemctl enable named
sudo ufw allow 53
sudo ufw reload
```

### Gitea (GitHub alternative)
```
sudo apt install git
sudo apt install mariadb-server

# Login to MySQL
sudo mysql -u root -p

# Add the git database
CREATE DATABASE gitea;
GRANT ALL PRIVILEGES ON gitea.* TO 'gitea'@'localhost' IDENTIFIED BY "Root";
FLUSH PRIVILEGES;
QUIT;

# Install gitea
sudo wget -O /usr/local/bin/gitea https://dl.gitea.io/gitea/1.16.7/gitea-1.16.7-linux-amd64 
sudo chmod +x /usr/local/bin/gitea
gitea --version

# Create gitea user
sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' --group --disabled-password --home /home/git git
sudo mkdir -pv /var/lib/gitea/{custom,data,log}
sudo chown -Rv git:git /var/lib/gitea
sudo chown -Rv git:git /var/lib/gitea

sudo mkdir -v /etc/gitea
sudo chown -Rv root:git /etc/gitea
sudo chmod -Rv 770 /etc/gitea
sudo chmod -Rv 770 /etc/gitea

# Append code to the service file
sudo nano /etc/systemd/system/gitea.service

[Unit]
Description=Gitea
After=syslog.target
After=network.target
[Service]
RestartSec=3s
Type=simple
User=git
Group=git
WorkingDirectory=/var/lib/gitea/

ExecStart=/usr/local/bin/gitea web --config /etc/gitea/app.ini
Restart=always
Environment=USER=git HOME=/home/git GITEA_WORK_DIR=/var/lib/gitea
[Install]
WantedBy=multi-user.target

# Start gitea
sudo systemctl start gitea
sudo systemctl status gitea
sudo systemctl enable gitea

# Access gitea
http://localhost:3000

# If you ever need to change settings like DOMAIN
sudo nano /etc/gitea/app.ini

```


Note: I put Gitea behind Nginx Proxy where it was listening to port 3000 and redirecting to 443 for SSL with a cert. The problem was that the URLs of the repos still had port 3000 so I would get command line errors when working with repos.

The fix was to edit the /etc/gitea/app.ini file and change the base URL to a normal port 80 URL. e.g., I deleted the :3000 from the base url.