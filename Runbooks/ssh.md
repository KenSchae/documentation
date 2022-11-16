# Secure Shell (SSH)

SSH is a network protocol for creating a secure connection from a client to a server. 

## Client-side setup

### On the server-side

In your home folder create an ssh folder

```
mkdir ~/.ssh && chmod 700~/.ssh
```

### Create SSH keys on your client

```
ssh-keygen -t rsa -b 4096 -C "email@domain.com"
```

### Copy key to a remote server

On Windows execute the following command to copy the public key up to the server.

```
scp $env:USERPROFILE/.ssh/id_rsa.pub username@ip_address:~/.ssh/authorized_keys
```

On Linux execute the following

```
ssh-copy-id username@ip_address
```

When you copy the key over, the server will challenge for your server password. After this initial step, the server will accept your keys as the login and you will not be challenged for the password.

### Copy key to Azure DevOps

Note about git: I have noticed that you need to generate keys for both your user account and root. 

### Login to remote server using your key

`ssh username@ip_address`