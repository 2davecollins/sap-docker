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
$ docker ps         list running containers
$ docker ps -a      list all containers
$ docker logs       show the container outputs
$ docker inspect [container]
$ docker container stop [id]
$ docker stop $(docker ps -aq)
$ docker container rm  [ID] [1d] [id]
$ docker container rm -f [ID]
$ docker rm $(docker ps -aq)
```
## docker images information
```
$ docker images
$ docker pull [IMAGE]

$ docker inspect [image]
$ docker rmi [image]
$ docker rmi $(docker images -a -q)

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
