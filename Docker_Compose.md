Start OpenVPN and open Proxmox from the Cyber Studies Portal

Create a VM running Ubuntu 22.04 (not 24.10 because it stopped being supported in July 2025)

Connect to the Network!
IPv4 : 10.30.76.6
Netmask : 255.255.255.0
Gateway : 10.30.76.1
DNS : 1.1.1.1

#Install_Docker

Here is the guide I followed: [(https://docs.docker.com/engine/install/ubuntu/)]

Here are the commands I an, in order:

```for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

Deleted any previous docker installations

```# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Setting up the apt repo

```sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
installation

```sudo systemctl status docker
```
ensure docker is running

```sudo docker run hello-world
```
check that installation was completed correctly

#Docker_Compose_GitLab

I followed this guide: [(https://medium.com/@BuildWithLal/gitlab-setup-using-docker-compose-a-beginners-guide-3dbf1ef0cbb2)]

the compose.yml file (with minimal edits):
``` 
services:  
  
gitlab-server:  
image: 'gitlab/gitlab-ce:latest'  
container_name: gitlab-server  
ports:  
- '8088:80'  
environment:  
GITLAB_ROOT_EMAIL: "email@email.com"  
GITLAB_ROOT_PASSWORD: "12345678"  
volumes:  
- ./gitlab/config:/etc/gitlab  
- ./gitlab/data:/var/opt/gitlab
```

```sudo docker compose up
```
starts the container

```sudo docker compose down
```
ends the container
