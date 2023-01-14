# docker-compose

These scripts will require to have docker installed.

## Install on Linux

How to [Install Docker Engine](https://docs.docker.com/engine/install/ubuntu/)

Recommend: [Docker post-installation steps](https://docs.docker.com/engine/install/linux-postinstall/)

How to [Install Portainer](https://docs.portainer.io/start/install/server/docker/linux) (bonus)

## Upgrade Portainer CE

We will upgrade using docker on Linux. You will need shell access and the ability to execute docker commands.

`docker stop portainer`

`docker rm portainer`

`docker pull portainer/portainer-ce:latest`

`docker run -d -p 9000:9000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest`
