# Docker

## Install Docker

```bash
sudo apt-get update

# install container runtime
sudo apt-get install -y gnupg2 software-properties-common ca-certificates
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y containerd.io docker-ce docker-ce-cli

sudo mkdir -p /etc/systemd/system/docker.service.d

sudo tee /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl enable docker

sudo sysctl --system

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt-get update
sudo apt-get install -y containerd.io

sudo mkdir -p /etc/containerd

containerd config default  /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
```

Settings

```bash
#sudo groupadd docker
sudo usermod -aG docker $USER

sudo chmod 666 /var/run/docker.sock
```

## Commands

Basic commands

```bash
docker run hello-world

docker pull alpine
docker pull alpine:3.7
docker run -it alpine:3.7 sh

docker ps
docker exec -it ba50145aa61b sh
```

Util commands

```bash
#process docker in status
docker stats

#excecute with environment variable
docker exec -e "TERM=xterm-256color" -w /workspace -it devcontainer bash
```

Images

```bash
docker run -ti ubuntu /bin/bash
docker ps -a | head
docker commit 6a15bc5c383f
docker image ls

docker image tag 6a15bc5c383f mydocker
docker commit d4f361137c7b
docker tag d4f361137c7b mydocker:1.1
docker run mydocker figlet hellow

docker build -t mydocker:1.2 .
docker run mydocker:1.2 figlet hellow
docker image history 0e3d2e901a67
```

Volumes

```bash
docker run -d nginx:1.15.7
docker ps
docker exec -it ad1b98d0d7e5 bash
docker stop ad1b98d0d7e5
docker run -v /root/notes/3.docker/index.html:/usr/share/nginx/html/index.html:ro -d nginx:1.15.7
docker ps
docker exec -it 924ef99d8a08 bash

docker ps
docker stop 924ef99d8a08
docker run -v /root/notes/3.docker/index.html:/usr/share/nginx/html/index.html:ro -d -p 8889:80 nginx:1.15.7
```

Docker-Compose

```bash
docker run -e MYSQL_ROOT_PASSWORD=mypass -e MYSQL_DATABASE=mydb -v /root/notes/4.docker/mysql-data:/var/lib/mysql -d mysql:8.0.13

docker-compose up -d
docker ps
```

Networking

```bash
#create network
docker network create --subnet 10.1.0.0/16 --gateway 10.1.0.1 --ip-range 10.1.4.0/24 devops_bridge

#connect docker to network
docker network connect devops_bridge hhtpd
docker network connect devops_bridge proxy
docker inspect devops_bridge

#list network for dockers
docker network ls
```

Clean

```bash
#clear docker not used
docker system prune

#remove all images
docker rmi $(docker images -a -q)
```

Export and Import

```bash
#export docker
docker export api_1 > api.tar #api_1 is name docker

#import docker
docker import api.tar
```

Inspect

```bash
docker image inspect --format '{{json .}}' mongo-express | jq
```
