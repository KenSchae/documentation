# Install Ubuntu Server

This guide assumes that the server is a plain-vanilla installation of Ubuntu Server. You can select the option to install OpenSSH Server during the installation wizard or follow the instructions below to install it post-installation. This guide assumes that you did not select any of the Featured Server Snaps during installation.

## Post Installation

Once installation is complete run update and upgrade to ensure that you have the latest version of the server installed along with all of its patches.

```
sudo apt update
sudo apt upgrade
```

## Install OpenSSH Server

These days everything should be done over SSH. Install the server so that you can connect to it using this standard protocol rather than FTP or RDP.

```
sudo apt install -y openssh-server
```
## Install GIT

If you will be editing or developing anything in this server, install GIT. GIT is the source control tool of choice.

```
sudo apt install -y git
```

## Install the Lynx browser

Lynx is a command-line browser. This guide assumes that you are installing Ubuntu server without a GUI, therefore you will need lynx to browse the web or to browse sites on this server (e.g., localhost).

```
sudo apt install -y lynx
```

