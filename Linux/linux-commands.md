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

### Copy Files and Folders to a Directory
```
cp -a /source/. /dest/
```
-a preserves all file attributes
. copies all files and folders including hidden

## Networking

### Network information
```
ifconfig -a
```

