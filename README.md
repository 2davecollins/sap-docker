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

## docker information
```
$ docker version
$ docker info

```