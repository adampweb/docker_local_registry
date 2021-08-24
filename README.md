# Private Docker Registry
___

## Introduction
I want to create a complete but clarify environment with Docker for web development for myself. 
This local private Docker registry is an important part of minimal requirements which are 
defined by myself. Currently, I have a strong beginner practice of Docker therefore one of my goals 
is to minimalist pulls from official Docker hub that cause lower network traffic therefore speed of 
work may be higher.


## Networking
Before created this [container](https://docker-curriculum.com/#what-are-containers-) I already
created a custom **[Docker network](https://docs.docker.com/network/)** with **devnet** name and IP-address range 
defined by **172.18.0.0/24**.

## Volumes
Only one [volume](https://docs.docker.com/storage/volumes/) defined for this container (directory name is **data**).
This directory will store the custom and pulled images from [Docker Hub](https://hub.docker.com/)

## How it works?
[Download and install Docker Engine](https://docs.docker.com/get-docker/)

[Download and install Docker Compose](https://docs.docker.com/compose/install/)

Create a directory for the project (in my case I defined a separated drive partition for Docker projects. Example: **/media/adam/DOCKER/DOCKER_REGISTRY**. 

> I used uppercase directory names for common-used projects, e.g.: Docker Registry, MySQL, phpMyAdmin, etc...). 
> 
> `mkdir -p /media/adam/DOCKER/DOCKER_REGISTRY`

Create a directory for the **volume** 

`mkdir -p /media/adam/DOCKER/DOCKER_REGISTRY/data`

Create a file with the following content
```yaml
version: '3.8'

services:
  registry:
    image: registry:2
    container_name: local_registry
    ports:
    - "5000:5000"
    networks:
      devnet:
        ipv4_address: 172.18.0.2
    environment:
      REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
    volumes:
      - ./data:/var/lib/registry
    restart: always

networks:
  devnet:
    external: true
```

Open the project root directory and create the container with the next command:

`docker-compose up -d`

If all right now when run `docker ps -a` command the running container is included in the resulting table.

## Sources
* [How To Set Up a Private Docker Registry on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-18-04)
* [docker docs](https://docs.docker.com/)
* [Deploy a registry server](https://docs.docker.com/registry/deploying/)