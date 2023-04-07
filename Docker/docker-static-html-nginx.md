# Docker for static html using Nginx

## Introduction
Use this runbook for your web development rather than setting up a whole Apache server.

## The Dockerfile
```
FROM nginx:alpine

COPY . /usr/share/nginx/html

EXPOSE 80
```

## Build the image
name:tag
`docker build -t myproject:v1 .`

## Run a container from the image
`docker run -d -p 8080:80 --name myproject myproject:v1`

## View the site
Open a browser at `http://localhost:8080`