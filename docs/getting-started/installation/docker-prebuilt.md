# Please read carefully
**⚠️ Warning**: It is your responsibility to ensure you protect your system from intruders/attacks. These commands and permissions are just examples used to get Wavelog up and running and are not a guide on how to achieve a secure system. You should review these permissions after installation and make appropriate changes if you determine that finer-grained access control is needed.

**⚠️ Warning**: Docker support is highly experimental, you may run into different issues when updating your existing setup, manual intervention may be needed to align tables and/or config files to the new version.

# Instructions

Docker images are now built automatically at every release, you can find them on [Docker HUB](https://hub.docker.com/r/xxxxxx/wavelog/tags). These are preconfigured images based on PHP7.4 and they include the latest stable release of Wavelog

# Prerequirements

- An available MySQL instance reachable from local network
    - An example setup can be found [here](https://github.com/wavelog/Wavelog/wiki/Building-a-Docker-image-from-scratch#mysql-and-phpmyadmin-stack)

# Docker setup

## Create persistent volumes

Some data needs to be retained locally (such as QSL cards, etc), so we will create four volumes to store such data

```sh
docker volume create wavelog-config
docker volume create wavelog-backup
docker volume create wavelog-uploads
```

## Execute Wavelog

Now that the volumes are created we can run the final image

```sh
docker run -v wavelog-config:/var/www/html/application/config -v wavelog-backup:/var/www/html/application/backup -v wavelog-uploads:/var/www/html/application/uploads --name wavelog -p 8086:80 xxxxxx/wavelog
```

This will expose the port 8086 on the local machine to access and configure the Wavelog instance!

## Final configuration

You can now access the WebPage to configure Wavelog at `https://ipaddress:8086` and proceed with the normal setup