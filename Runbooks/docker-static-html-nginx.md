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
`docker build -t myproject:v1 .`

## Run a container from the image
`docker run -d -p 8080:80 myproject:v1`