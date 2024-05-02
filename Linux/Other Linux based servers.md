Optional Maintenance

### Create new sudo account
Ubuntu Server does this by default
```
sudo adduser {username}
sudo usermod -aG sudo {username}
```

### Disable root
Ubuntu Server does this by default
```
sudo passwd -l root
```

### Change HOSTNAME
```
sudo hostnamectl set-hostname {hostname}
hostnamectl

sudo nano /etc/hosts
/* change hostname in hosts file */
```

## Optional Services

### Install xRDP
If you need to use RDP to access a desktop environment
```
sudo apt install xrdp
sudo systemctl enable xrdp
```

### Install GIT
```
sudo apt install git
```

### Install Guest Agent for Proxmox
Applies only if this box is in a Proxmox server

1. Open Proxmox
2. Click Options
3. Turn on QEMU Agent

```
sudo apt install qemu-guest-agent
sudo shutdown 
```

### Install Webmin
```
sudo apt install software-properties-common apt-transport-https
sudo wget -q http://www.webmin.com/jcameron-key.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://download.webmin.com/download/repository sarge contrib"
sudo apt install webmin
sudo ufw allow 10000/tcp
sudo ufw reload
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

