
## Server Management
### Install Webmin

WebMin is a web app that provides a number of server management features. Everything it does can be done from the command line, but it tends to make things easier.

```
sudo apt install software-properties-common apt-transport-https
sudo wget -q http://www.webmin.com/jcameron-key.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://download.webmin.com/download/repository sarge contrib"
sudo apt install webmin
sudo ufw allow 10000/tcp
sudo ufw reload
```

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

## Networking

### Network information
```
ifconfig -a
```

## File System Management

### Copy Files and Folders to a Directory
```
cp -a /source/. /dest/
```
-a preserves all file attributes
. copies all files and folders including hidden
