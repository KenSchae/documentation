# Linux Stuff

## Install distro

## Post install
```
apt update
apt upgrade
```

## Automatic Updates
To do updates manually. Don't do this on Production servers because you should have better planning to upgrade you key servers.

```
apt update
apt upgrade
```

Install a utility that will perform auto-upgrades

```
apt install unattended-upgrades
```

Configure the utility

```
dpkg-reconfigure --priority=low unattended-upgrades
```

## Create new sudo account
```
sudo adduser {username}
sudo usermod -aG sudo {username}
```

## Disable root

```
sudo passwd -l root
```

## SSH Logins

### Install openssh
```
sudo apt install openssh-server
```

### Setup key generated login
```
ssh-keygen
```

Go to client machine
```
ssh-keygen
ssh-copy-d username@ipaddress
```

### Lockdown Logins
Edit the following file in nano on the server:

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

## Static IP

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
        gateway: 192.168.5.99
        nameservers:
            addresses: [192.168.5.25]

sudo netplan apply
```

## Fix LVM
```
sudo lvm
lvm > lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
exit

sudo resize2fs /dev/ubuntu-vg/ubuntu-lv
```

## Change HOSTNAME
```
sudo hostnamectl set-hostname {hostname}
hostnamectl

sudo nano /etc/hosts
/* change hostname in hosts file */
```

## Change timezone
```
timedatectl
timedatectl list-timezones
sudo timedatectl set-timezone America/Chicago
```

## Install Guest Agent for Proxmox
Applies only if this box is in a Proxmox server

1. Open Proxmox
2. Click Options
3. Turn on QEMU Agent

```
sudo apt install qemu-guest-agent
sudo shutdown 
```

## Configure Firewall
```
sudo ufw status
sudo ufw default allow outgoing
sudo ufw default deny incoming
sudo ufw allow ssh
sudo ufw enable
```
## Install fail2ban
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