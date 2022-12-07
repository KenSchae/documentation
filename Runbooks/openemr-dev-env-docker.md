# OpenEMR Development Environment (Docker)

## TABLE OF CONTENTS


## BUILD DEVELOPMENT SERVER
The OpenEMR video series (and many home lab tutorials) walk you through creating a Linux VM to develop with. The assumption in these videos is that you will use the VM as a workstation AND as a server: everything in one box. I don't like that approach. My preference is to create the VM as a server and then connect to the server from a workstation. 

1. Build a Linux Server VM on either an internet accessible private cloud, or on one of the major cloud providers (e.g., Azure, AWS, etc.)
2. Install OpenSSH (if you did not select it during installation, Azure installs it automatically)
3. Setup your work computer with ssh keys to login to server

## CONNECT WORKSTATION TO SERVER
1. In VSCode add Remote - SSH extension
2. Open SSH Config file and add entry for the server
```
Host arbitrary-name-that-displays-in-vscode
  HostName ip-address-or-dns-hostname 
  User username
  Port portnumber-9091
```
3. Connect VSCode to server and open a new terminal window

## CONFIG SERVER
1. Install Git
2. Install Docker
3. Install Docker-compose
4. Install OpenEMR-cmd

## INSTALL OPENEMR-CMD
1. Go to openemr/openemr-devops/utilities on GitHub
2. Follow installation instructions at GitHub

## FORK THE GITHUB REPO
1. Create GitHub account
2. Configure Git and create SSH key on server
3. Register SSH key with GitHub
4. Fork the OpenEMR repo into your own account
5. Clone your fork into the server

## START THE DOCKERS
1. `cd ~/git/openemr/docker/development-easy`
2. `openemr-cmd up`
3. Find the donuts in the breakroom
4. Enjoy the Phil Collins easy listening tune that is playing
5. `docker ps` there are 5 dockers for the OpenEMR environment
6. `openemr-cmd dl` keep running this until the docker is done building (e.g.Apache is running)

## TEST OPENEMR
1. Open a browser
2. http://hostname-or-ip-of-server:8300
3. admin/pass
4. http://hostname-or-ip-of-server:8310 (opens phpmyadmin)
5. root/root

## SLING SOME CODE
1. Make a change
2. Refresh browser or open the site again
3. `git checkout -b code-slinging`
4. `git commit -a -m "slinging"`
5. `git push origin code-slinging`
6. Go to GitHub
7. Click the create pull request to create a pull request to the main openemr

## SAMPLE PATIENT DATA WITH SYNTHEA
1. `openemr-cmd irp 10` 
2. By now the donuts are all gone except for that gross coconut covered one
3. Actually I kind of like the coconut donuts
4. Let's be real, I like all donuts
5. I am the Homer Simpson of full stack developers

## REFERENCES
- [OpenEMR Development Docker Environment](https://github.com/openemr/openemr/blob/master/CONTRIBUTING.md#starting-with-openemr-development-docker-environment)
- [Presentation Slides](https://docs.google.com/presentation/d/13VhN_uL-YptFtM1qv3pJ6DwhMnz9tNrAtUSbMEpHMLU/edit#slide=id.p)
- [OpenEMR Cmd](https://github.com/openemr/openemr-devops/tree/master/utilities/openemr-cmd#openemr-cmd-documentation)