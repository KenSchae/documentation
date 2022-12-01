# DOCKER BASICS

## Table of Contents
1. [Introduction](#introduction)
2. [Installation](#ubuntu-installation)
3. [Concepts](#concepts)
4. [Images](#images)
5. [Managing Images](#managing-images)
6. [Containers](#containers)
7. [Managing Containers](#managing-containers)

## INTRODUCTION

This file contains notes and code examples for using Docker. 

I am a big fan of Academind on uDemy and YouTube. Their classes on eDemy are content rich and take you from noob to expert. Maximillian works from hands-on examples that you will actually use in IT. I highly recommend purchasing his course on Docker https://www.udemy.com/course/docker-kubernetes-the-practical-guide/. 

## UBUNTU INSTALLATION

hmmmm, this looks like it should be a bash script.

### Base Install
As usual, update your packages

`sudo apt update`

Prepare Ubuntu for the Docker installation

`sudo apt install apt-transport-https ca-certificates curl software-properties-common`

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg`

`echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`

Update the packages again

`sudo apt update`

Now do the install from Docker

`apt-cache policy docker-ce`

`sudo apt install docker-ce`

Make sure it is running

`sudo systemctl status docker`

### Put yourself in the Docker group
Do this so you don't have to use sudo all the time

`sudo usermod -aG docker ${USER}`

You can reboot to make it take effect or...

`su - ${USER}`

## CONCEPTS

### Images

Images are the template (instructions) that define the parameters of the container. The image is used to build containers and a single image can build many containers.

### Containers

Containers are packages that contain the application and the environment in which the application runs. 

## IMAGES

### Define your own image using a Dockerfile

Create a Dockerfile in the project directory. This file is a set of instructions for building a container.

```
# the baseImage is either a name from DockerHub or is the 
# name of an Image on your system
FROM node 

# tell Docker where to run commands from
WORKDIR /app

# tell Docker which files go into the image
# COPY localFolder imageFolder 
COPY . /app

# do some node stuff
RUN npm install

# show the port to the world outside of Docker so that the 
# web pages are visible
EXPOSE 80

# these commands are executed when a container is instanced
CMD ["node", "server.js"]

```

### Build an image from the Dockerfile. 

Using this command results in a random Image ID that you use to run a container. This is not the way.

`docker build .`

### Assign a Name:Tag to the image

Assign a name and tag to the image because you are not a barbarian. 

`docker build -t name:tag .`


## MANAGING IMAGES

### List out the images installed on this server
`docker images`

### Remove an image from this server
Can't remove images that have containers (started and stopped)
`docker rmi {imagename}`

### Remove all dangling (untagged) images
`docker image prune`

### Review detailed information about the image
`docker image inspect {imageid}`

## CONTAINERS

### Run a container from a DockerHub image

`docker run node`

This command will create and run a container instance from the image. In this example it is using the office nodejs image to create a running instance in a container. Note: this command as it is doesn't do much. 

`docker run -it node`

Adding the -it flag creates an interactive terminal instance where you can interact with the instance of node.

### Running a container from your own image

After the image is built you will get an ID for the image. Use the ID to start the container. If you are a good dev, then you assigned a name:tag and you use that instead.

`docker run -p 3000:80 e5a737911cca`


## MANAGING CONTAINERS

### Running a container
Get a list of all parameters for running a container
`docker run --help`

Common parameters for running a container
|Switch | Description |
|-------|-------|
|-p     |(port) map a port|
|-d     |(detached mode) run in the background|
|-it    |(interactive mode) not -d|
|--rm   |(remove) the container when the service is stopped|
|--name |(name) give the container a name|
|-v     |(volume) assigns a named volume to the container| 

`docker run -p 3000:80 -d --rm --name goalsapp goals:latest`

Expose a terminal for console applications. See the Python example.
`docker run -it {containername}`
`docker start -a -i {containername}` 

### List all running containers
`docker ps`

### List all containers (running and not running)
`docker ps -a`

### Stop a container  
`docker stop {containername}`

### Start a stopped container  
This command will start a container in detached mode.
`docker start {containername}`

### Remove a stopped container (can't remove a running container)
`docker rm {containername}`

### Remove ALL stopped containers
`docker container prune`

### Attach to a running container
Use this if you are running in detached mode and need to attach to see content logged to the console.
`docker attach {comtainername}`

### View logs from a container
logs are the console output from a container. This command will let you see the content.
`docker logs {containername}`

### Copy files into or out of a running container
Copy in
`docker cp localpath/. {containername}:/pathincontainer`
Copy out
`docker cp {containername}:/pathincontainer/. localpath`


## VOLUMES

Volumes store persistent data that survives after a container shuts down. These are folders on the host machine that are mounted in the container. 

Volumes are managed by Docker. This means that we do not know where the volume is located and have no ability to manage them. Bind mounts solve that problem.

### Add a volume to the container

This command creates an anonymous volume. Anonymous volumes are not persisted when the container is removed.

`VOLUME ["{path}"]`
`VOLUME ["app/feedback"]`

Named volumes persist data when the container is removed. Named volumes are NOT defined in the Dockerfile, rather it is done at the command line when running the container.

`-v {volume name}:{mount location in container}`

`docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes`

### List volumes

`docker volume ls`

### Troubleshooting

`docker logs {appname}`

## Bind Mounts

Like named volumes but we have management control over the file system. Bind mounts are not created in the Dockerfile. Bind mounts are created from terminal.

A bind mount is created by adding a second -v command. Should use quotes to protect the absolute path. 

`-v "{absolute path on host file system}:{mount location in container}"`

`docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/kschaefer/repos/webdev/docker/data-volumes-01-starting-setup:/app" feedback-node:volumes`

If you mount a volume to the app folder in the way that this command above does, then the mount will overwrite everything that the COPY and RUN commands do in the Dockerfile. This command is overwriting the container folder with the contents of the local host folder. Since the local host does not have the same npm dependencies as the container then we get an issue.

The solution is to use an anonymous volume. In the following command the folder that is defined in the anon volume is protected.

`docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/kschaefer/repos/webdev/docker/data-volumes-01-starting-setup:/app" -v /app/node_modules feedback-node:volumes`

🛸 Why do  this? Now we can make changes to the HTML files and we do NOT have to rebuild the container.

### Read Only Volume
When using a Bind Mount, the local drive that is mapped to the container is writable by the container. Since we probably don't want the container writing back to our source-code marking the volume as read-only is a good idea. Do this by adding :ro to the end of the volume declaration.

`docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "/home/kschaefer/repos/webdev/docker/data-volumes-01-starting-setup:/app:ro" -v /app/node_modules feedback-node:volumes`

You can add volume declarations after the bind-mount to make subfolders writable if they need it. Use anonymous folders to do this. In the above example, all of app is read only but node_modules can be written by the container.

## References

[Docker & Kubernetes: The Practical Guide](https://www.udemy.com/course/docker-kubernetes-the-practical-guide/)