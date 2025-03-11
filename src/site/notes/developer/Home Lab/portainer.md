---
{"dg-publish":true,"permalink":"/developer/home-lab/portainer/","dgPassFrontmatter":true}
---

#docker 
# [Portainer](https://www.portainer.io/)
Docker and Kubernetes Management. A no-code solution for spinning up containers. I usually use it to start / stop / log / shell into containers easily

## connections
- one

## installation
### [[developer/Home Lab/Docker\|Docker]]
1. `./compose.yml`
```
version: "3.2"

services:
  portainer:
    image: portainer/portainer-ce:2.11.1
    container_name: portainer
    restart: always
    ports:
      - 8000:8000
      - 9000:9000
      - 9443:9443
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./portainer_data:/data

# networks:
#   default:
#     external:
#       name: nginx-prox-mgmt-3_default
```

2. `./upgrade.sh`
```
#! /bin/bash
echo "[[ upgrading portainer | upgrading portainer ]]"

echo "[[ stop portainer docker | stop portainer docker ]]"
sudo docker stop portainer

echo "[[ remove old portainer docker | remove old portainer docker ]]"
sudo docker rm portainer

echo "[[ sudo docker-compose u -d | sudo docker-compose u -d ]]"
cd /home/<username>/docker/portainer

sudo docker-compose up -d
```

### upgrade a container
[How to Update a Docker Container using Portainer - WunderTech](https://www.wundertech.net/how-to-update-a-docker-container-using-portainer/)
[video tutorial - WunderTech](https://www.youtube.com/watch?v=CK9blZF1lFo)
1. Select **Containers** http://YOUR_DOMAIN.lan:9000/#!/2/docker/containers, then press <button>⬜Stop</button>  the container that you’d like to update.
2. Select into the individual container,(you'll see `Container details` at the top of the page) and you’ll see a button named <button>🔄Recreate</button> By selecting this button, 
> [!error] make sure your critical data is mapped to a volume. That means bye bye to any data only living on RAM
4. Select **Pull latest image** toggle, then **Recreate**.
5. When this process is finished, the container will be recreated with the latest image. Select the container and **Start** it. The status will change to **running**
## Alternatives
Portainer has been pushing heavy on their enterprise/paid models within the app. I may switch over to [louislam/dockge: A fancy, easy-to-use and reactive self-hosted docker compose.yaml stack-oriented manager (github.com)](https://github.com/louislam/dockge)as it seems to be a slimed down version of all the stuff I'd use in a container manager