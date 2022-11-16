# Linux Stuff

## Hardening

1. Automatic Updates (Non-prod only)
2. SSH Logins
3. Lockdown logins
4. Firewall

### Automatic Updates

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

### SSH Logins

### Lockdown Logins

Edit the following file in nano on the server:

```
sudo nano /etc/ssh/sshd_config
```

Make the following changes:

1. Change the **Port** to something other than 22
2. Uncomment **AddressFamily** 
3. Change AddressFamily to **inet**
4. Change PermitRooLogin to **no**
5. Change PasswordAuthentication to **no**

Save the file and restart the ssh service

```
sudo systemctl restart sshd
```