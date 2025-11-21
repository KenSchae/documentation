## Table of Contents
1. [[#Introduction]]
2. [[#Pre Install]]
	1. [[#Select distro]]
	2. [[#Download ISO]]
3. [[#Installation]]
4. [[#Post Install]]
	1. [[#Update && Upgrade]]
	2. [[#Automatic Updates]]
	3. [[#Static IP]]
	4. [[#Install openssh]]
	5. [[#Fix LVM]]
	6. [[#Change timezone]]
	7. [[#Configure Firewall]]
	8. [[#Install fail2ban]]
5. [[#Optional Base Software]]
6. [[#Conclusion]]
## Introduction

This guide focuses on installing Linux on a hypervisor of some sort. At least, that is how I most often use this guide: as a checklist for building out a server, dev environment, or something else Linux related. That said, these steps are valid for building out a box as well.

As far as a hypervisor, I have an old HP ProLiant server that is running Windows Server 2019 with Hyper-V installed and I have a newer ProLiant that is running Proxmox. This guide assumes that you know how to create a VM using your hypervisor and how to start the server from an ISO.
## Pre Install

### Select distro

Asking which distro of Linux is the best one is like asking which super-hero is the best in a comic shop. The selection of Linux distro hinges upon your requirements and what you intend to accomplish with it. 

The following four are a handful of distros that have remained popular for more than a couple of years. Finding tutorials, YouTube channels, and other assistance is easy for all of these.

1. [Debian](https://www.debian.org/)
2. [Ubuntu](https://ubuntu.com/)
3. [Mint](https://linuxmint.com/)
4. [Fedora](https://fedoraproject.org/)

*Note: 11/21/25 Lately I have been building out local LLM servers on Pop!_OS Linux because this distro has the NVidia drivers built in. It is based on Ubuntu and is a good distro. If you are building out a box that has NVidia, you may want to go with this.*

- [Pop!_OS]([Pop!_OS by System76](https://system76.com/pop/))

### Download ISO

When downloading the OS, select an ISO file and try to find the latest stable release or an LTS (Long Term Support) version of the OS. You may select the niche options but these assume that you know what you are doing.

Store the ISO in a place that is accessible to the hypervisor where you will install Linux. I tend to keep a file-share with all of my ISOs. 

## Installation

1. Create a VM in your hypervisor
2. Configure the VM "hardware"
3. Start the VM from the ISO
4. Follow the distro's installation steps

- [Install Ubuntu Server]([Install Ubuntu Server | Ubuntu](https://ubuntu.com/tutorials/install-ubuntu-server#1-overview)
- [Install Pop!_OS]([Installing Pop!_OS - System76 Support](https://support.system76.com/articles/install-pop)

## Post Install

### Update && Upgrade

Always run these two command immediately after completing the installation because fixes have likely been released since the ISO was created. You should also run these two commands immediately before installing a major piece of software on your instance. Lastly, you should run these on a regular basis to keep your box up to date.

```
apt update
apt upgrade
```

### Automatic Updates

Some hot fixes and security patches should not wait around until you finally decide to actually run the update and upgrade commands above. For this reason, setup unattended upgrades for the important stuff.

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
        addresses: [192.168.5.50/24]
        nameservers:
            addresses: [192.168.5.25,8.8.8.8]
        routes: 
            - to: default
              via: 192.168.5.99

sudo netplan apply
```

*The filename of the netplan configuration may not be the same as in the example above.*

### Install openssh
Ubuntu installation has the option to install OpenSSH as part of the OS installation. You only need to run the install if you skipped this step.
```
sudo apt install openssh-server
ssh-keygen
```
#### SSH on Client machine
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
#### Lockdown Logins to SSH only
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

This seems to be an Ubuntu thing. Most of the other distros don't need this but it doesn't hurt to check your volumes. Basically, you are making sure that Linux is using the whole hard drive rather than part of it.

*Note: 11/21/25 This does not seem to be an issue anymore, the installer asks to use the whole disk.*

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


## Optional Base Software

### Install xRDP
If you need to use RDP to access a desktop environment. Most of the time, you will use SSH to access the command line.
```
sudo apt install xrdp
sudo systemctl enable xrdp
```

### Install GIT
```
sudo apt install git
```

## Conclusion

At this point you have a solid Linux server. From here move on to install the individual server components that you need.

[[Install Server Software on Base Linux]]