# Docker Commands

## install docker on ubuntu 16 Okeanos

### to get the latest version of docker add the GPG key from Docker repository

### Make sure installation from that repository

```
$ sudo apt update
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update

$ apt-cache policy docker-ce


result should be as follows

docker-ce:
  Installed: (none)
  Candidate: 18.06.1~ce~3-0~ubuntu
  Version table:
     18.06.1~ce~3-0~ubuntu 500
        500 https://download.docker.com/linux/ubuntu xenial/stable amd64 Packages


install docker

$ sudo apt-get install -y docker-ce
$ sudo systemctl status docker

$ sudo usermod -aG docker ${USER}
$ su - ${USER}
$ id -nG


or

$ sudo usermod -aG docker username

$ docker [option] [command] [arguments]

$ docker run hello-world

$ docker search [name]

```

### install docker-compose
latest release
[docker-compose latest](https://github.com/docker/compose/releases)

```
$ sudo curl -L https://github.com/docker/compose/releases/download/2.16.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose
$ docker-compose --version

```

## docker container information
```
$ docker version
$ docker info
$ docker ps         # list running containers
$ docker ps -a      # list all containers
$ docker logs       # show the container outputs
$ docker inspect [container]
$ docker container stop [id]
$ docker stop $(docker ps -aq)
$ docker container rm  [ID] [1d] [id]
$ docker container rm -f [ID]
$ docker rm $(docker ps -aq)

$ docker container stats [NAME]

with git bash
$ winpty docker container run -it --name [NAME] nginx bash

$ docker container run -it --name ubuntu ubuntu
$ docker container run --rm -it --name [NAME] ubuntu
$ docker container exec -it mysql bash
$ docker container run -it alpine sh

```
## docker images information
```
$ docker images
$ docker pull [IMAGE]

$ docker inspect [image]
$ docker rmi [image]
$ docker rmi $(docker images -a -q)

```
## Networking
```
$ docker network ls
$ docker network inspect [NETWORK_NAME]
("bridge" is default)

$ docker network create [NETWORK_NAME]
$ docker network connect [NETWORK_NAME] [CONTAINER_NAME]
$ docker network disconnect [NETWORK_NAME] [CONTAINER_NAME]
$ docker network rm [NETWORK_NAME]

detatch network
$ docker network disconnect
```
## volumes
```
$ docker volume ls
$ docker volume prune

$ docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
$ docker volume inspect mysql-db

Run and be able to edit index.html file (local dir should have the Dockerfile and the index.html)
$ docker container run  -p 80:80 -v $(pwd):/usr/share/nginx/html nginx

```
## docker create
```
docker create [options] IMAGE
  -a, --attach               # attach stdout/err
  -i, --interactive          # attach stdin (interactive)
  -t, --tty                  # pseudo-tty
      --name NAME            # name your image
  -p, --publish 5000:5000    # port map
      --expose 5432          # expose a port to linked containers
  -P, --publish-all          # publish all ports
      --link container:alias # linking
  -v, --volume `pwd`:/app    # mount (absolute paths needed)
  -e, --env NAME=hello       # env vars
```
## docker exec
```
docker exec [options] CONTAINER COMMAND
  -d, --detach        # run in background
  -i, --interactive   # stdin
  -t, --tty           # interactive

```
## Sample container creation
```
NGINX:
$ docker container run -d -p 80:80 --name nginx nginx

APACHE:
$ docker container run -d -p 8080:80 --name apache httpd

MONGODB:
$ docker container run -d -p 27017:27017 --name mongo mongo

MYSQL:
$ docker container run -d -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=123456 mysql

EXAMPLE MYSQL 1
## Default bridge network

START MYSQL SERVER WITH CUSTOM ROOT PASSWORD
docker run \
    -e MYSQL_ROOT_PASSWORD=my-password \
    mysql

START PHPMYADMIN WITH PMA_HOST VARIABLE (over IP address)
docker run \
    -p 8080:80 \
    -e PMA_HOST=172.17.0.2 \
    phpmyadmin/phpmyadmin

## MYSQL EXAMPLE 2 
## Custom bridge network

CREATE CUSTOM BRIDGE NETWORK
docker network create mysql

START MYSQL SERVER WITH CUSTOM ROOT PASSWORD
docker run \
    --network mysql \
    -e MYSQL_ROOT_PASSWORD=my-password \
    --name mysql \
    -d mysql

START PHPMYADMIN WITH PMA_HOST VARIABLE (over DNS name - name of the container)
docker run \
    --network mysql \
    -p 8080:80 \
    -e PMA_HOST=mysql \
    -d phpmyadmin/phpmyadmin

## EXAMPLE 3
START PHPMYADMIN WITH PMA_HOST ON OKEANOS

START PHPMYADMIN WITH PMA_HOST VARIABLE (over DNS name - name of the container)
docker run \
    --network mysql \
    -p [OKEANOS-IP]:8080:80 \
    -e PMA_HOST=mysql \
    -d phpmyadmin/phpmyadmin

## EXAMPLE 4
# START MYSQL WITH PERSISTANT DATA IN LOCAL /DB/DATADIR
# CREATE directories on local machine mkdir db cd db mkdir datadit
docker run \
    --network mysql 
    -v $(pwd)/db/datadir:/var/lib/mysql 
    -e MYSQL_ROOT_PASSWORD=my-password 
    --name mysql 
    -d mysql


CREATE DATABASE DUMP
Most of the normal tools will work, although their usage might be a little convoluted in some cases to ensure they have access to the mysqld server.
A simple way to ensure this is to use docker exec and run the tool from the same container, similar to the following:

docker exec mysql sh \
    -c 'exec mysqldump --all-databases -uroot -p"$MYSQL_ROOT_PASSWORD"' \
    > /some/path/on/your/host/all-databases.sql

For restoring data. You can use docker exec command with -i flag, similar to the following:

docker exec -i some-mysql sh \
    -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < /some/path/on/your/host/all-databases.sql


REDIS:
CREATE NEW CUSTOM NETWORK
docker network create redis

LAUNCH REDIS CONTAINER
docker run \
    --name redis \
    --network redis \
    -d redis

LAUNCH REDIS-COMMANDER CONTAINER
docker run \
    --name redis-commander \
    --network redis \
    -p 8081:8081 \
    -e REDIS_HOST=redis \
    -d rediscommander/redis-commander

```
## Dockerfile
```
$ FROM              # The os used. Common is alpine, debian, ubuntu
$ ENV               # Environment variables
$ RUN               # Run commands/shell scripts, etc
$ EXPOSE            # Ports to expose
$ CMD               # Final command run when you launch a new container from image
$ WORKDIR           # Sets working directory (also could use 'RUN cd /some/path')
$ COPY              # Copies files from host to container

build image from same directory as Dockerfile
$ docker image build -t [NAME] .

Example
FROM nginx:latest                   # Extends nginx so everything included in that image is included here
WORKDIR /usr/share/nginx/html
COPY index.html index.html

Build
$ docker image build -t nginx-website

Run
$ docker container run -p 80:80 --rm nginx-website

```
## clean up
```
$ docker image prune             # unused images
$ docker image prune -a          # all not in use
$ docker system prune            # entire system
$ docker kill $(docekr ps -q )   # kill all running containers

```
# docker-compose
## Basic example
```
version: '2'
services:
    web:
    build: .            # build from Dockerfile
    context: ./Path
    dockerfile: Dockerfile
    ports:
        - "5000:5000"
    volumes:
        - .:/code
    redis:
        image: redis

```
### commands
```
$ docker-compose start
$ docker-compose stop
$ docker-compose pause
$ docker-compose unpause
$ docker-compose ps
$ docker-compose up
$ docker-compose down

```