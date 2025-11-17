# Wireguard (Project 3 CYB_3353)

## Pre-steps

Create droplet on Digital Ocean (created a droplet running ubuntu 24.04)

Install docker onto the droplet
I followed this guide: [https://docs.docker.com/engine/install/ubuntu/] (https://docs.docker.com/engine/install/ubuntu/)

## Running_Wireguard

I followed the guide linked here: [https://pimylifeup.com/wireguard-docker/] (https://pimylifeup.com/wireguard-docker/)

```sudo mkdir -p /opt/stacks/wireguard```
created a directory to host compose file

```cd /opt/stacks/wireguard```
move into newly created directory

```docker run --rm -it ghcr.io/wg-easy/wg-easy wgpw '<PASSWORD>'```
create salted and hashed password that will be stored in you compose file. When the container is running, you will be able to launch a web page that allows you to create clients and turn on/off the vpn for said clients. You will need this password to access this site. 

```sudo nano compose.yaml```
open nano to write the compose file

```
services:
  wg-easy:
    container_name: wg-easy
    image: ghcr.io/wg-easy/wg-easy

    environment:
      - <PASSWORD_HASH>
      - WG_HOST=<IPADDRESS>

    volumes:
      - ./config:/etc/wireguard
      - /lib/modules:/lib/modules
    ports:
      - "51820:51820/udp"
      - "51821:51821/tcp"
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.ip_forward=1
      - net.ipv4.conf.all.src_valid_mark=1
```
Here is the compose.yml file. 

You need to add your personal hashed password in the env variables. It should look like PASSWORD_HASH=yourpassword. Leave out the single quotes and everywhere there is a dollar sign in the hash, add another one beside it. 

For the WG_HOST, you will need to find the public ip associated with your droplet. One place to locate this is on the home page of your droplet on the digitalocean website.

```docker compose up -d```
start your container

```hostname -I```
copy the public ip address listed and open a new tab in your browser. In the search bar, type http://yourhostname:51821

This will take you to the web page where you will need to login in using your password.

Next create a client. The name doesn't matter.

On your phone, download the wireguard app, and scan the QR code to create a tunnel. 

Boom, the vpn on your phone is able to be actived through the app.

For you computer, youn need to download the .config file. On a mac, go to the app store and download the wireguard app. Import the .config file to activate and tunnel, and again, boom. The vpn is up and running. 
