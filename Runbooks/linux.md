# Linux Stuff

## Install distro

## Post install

### Update && Upgrade
```
apt update
apt upgrade
```

### Automatic Updates
```
apt install unattended-upgrades
dpkg-reconfigure --priority=low unattended-upgrades
```

### Static IP
```
sudo nano /etc/netplan/01-netcfg.yaml

network:
  version: 2
  renderer: networkd
  ethernets:
    ens18:
        dhcp4: no
        addresses:
          - 192.168.5.50/24
        routes: 
          -to: default
           via: 192.168.5.99
        nameservers:
            addresses: [192.168.5.25]

sudo netplan apply
```

### Install openssh
You can select this during Ubuntu install
```
sudo apt install openssh-server
ssh-keygen
```

### SSH on Client machine
LINUX client machine
```
ssh-keygen
ssh-copy-id username@ipaddress
```
Windows client
```
ssh-keygen
Get-Content $env:USERPROFILE\.ssh\id_rsa.pub | ssh <user>@<hostname> "cat >> .ssh/authorized_keys"
```

### Lockdown Logins to SSH only
```
sudo nano /etc/ssh/sshd_config
```

Make the following changes:
1. Change PasswordAuthentication to **no**
2. Change ChallengeResponseAuthentication to **no**

Save the file and restart the ssh service
```
sudo systemctl restart sshd
```

### Fix LVM
```
sudo lvm
lvm > lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
exit

sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
```

### Change timezone
```
timedatectl
timedatectl list-timezones
sudo timedatectl set-timezone America/Chicago
```

### Configure Firewall
```
sudo ufw status
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw enable
```

### Install fail2ban
```
sudo apt install fail2ban
sudo cp /etc/fail2ban/fail2ban.{conf,local}
sudo cp /etc/fail2ban/jail.{conf,local}
sudo nano fail2ban.local

loglevel = INFO
logtarget = /var/log/fail2ban.log

sudo nano /etc/fail2ban/jail.local

bantime = 10m
findtime = 10m
maxretry = 5
backend = systemd

sudo systemctl restart fail2ban

sudo fail2ban-client status /* use to review banned */
```

## Optional Maintenance

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
sudo mysql -u root -p

CREATE DATABASE gitea;
GRANT ALL PRIVILEGES ON gitea.* TO 'gitea'@'localhost' IDENTIFIED BY "Root";
FLUSH PRIVILEGES;
QUIT;

sudo wget -O /usr/local/bin/gitea https://dl.gitea.io/gitea/1.16.7/gitea-1.16.7-linux-amd64 
sudo chmod +x /usr/local/bin/gitea
gitea --version

sudo adduser --system --shell /bin/bash --gecos 'Git Version Control' --group --disabled-password --home /home/git git
sudo mkdir -pv /var/lib/gitea/{custom,data,log}
sudo chown -Rv git:git /var/lib/gitea
sudo chown -Rv git:git /var/lib/gitea

sudo mkdir -v /etc/gitea
sudo chown -Rv root:git /etc/gitea
sudo chmod -Rv 770 /etc/gitea
sudo chmod -Rv 770 /etc/gitea

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

sudo systemctl start gitea
sudo systemctl status gitea
sudo systemctl enable gitea

http://localhost:3000
```


### Logging with Prometheus