# Please read carefully
**⚠️ Warning**: It is your responsibility to ensure you protect your system from intruders/attacks. These commands and permissions are just examples used to get Wavelog up and running and are not a guide on how to achieve a secure system. You should review these permissions after installation and make appropriate changes if you determine that finer-grained access control is needed.

**⚠️ Warning**: Docker support is highly experimental, you may run into different issues when updating your existing setup, manual intervention may be needed to align tables and/or config files to the new version.

# Instructions

Altough this procedure is not fully recommended (as the image is statically build and you won't get automatic updates as described in the other installation methods), it is technically possible to create and instantiate a Docker Container with Wavelog.

These steps are written for a Linux machine but can be easily ported to any system.

Setup using Portainer is also possible and highly recommended (easier maintenance, see below)

- If you already have an existing MySQL server reachable by your network you can directly skip to the `Execute Wavelog` chapter
- If you are a beginner and just want to have the Wavelog software up and running in few steps please [read here](https://github.com/wavelog/Wavelog/wiki/Building-a-Docker-image-from-scratch#option-4-standalone-stack-recommended-for-beginners)


# Prerequirements
## Docker engine
Please refer to the official [Docker documentation](https://docs.docker.com/engine/install/)
## Docker compose
This plugin is usually included in Docker installations, if you need to set it up manually please refer to the [Docker documentation](https://docs.docker.com/compose/install/)
## MySQL
- A MySQL server must be reachable by the Wavelog container instance, this can be a container too, based on the [Linux setup requirements](https://github.com/wavelog/Wavelog/wiki/Installation) a quick and easy solution is using the `mysql:5.7` image from [Docker hub](https://hub.docker.com/layers/library/mysql/5.7/images/sha256-c4c526804552f6b4e8e124e182f5df4b09bf4bc88cba8a94adbd0a2ccb81dce6?context=explore)
- If you already have a MySQL server in your local network please skip the following chapter
### MySQL and PhpMyAdmin stack
If you don't have any MySQL server available on the network, a custom stack can be built containing both MySQL and PhpMyAdmin
1. Create a `mysql` folder with `mkdir mysql`
2. Move to that folder with `cd mysql` and create a `docker-compose.yml` file using your preferred editor such as `nano docker-compose.yml`
3. Paste the following code
```yaml
version: '3'
 
services:
  db:
    image: mysql:5.7
    container_name: wavelog-mysql
    environment:
      MYSQL_ROOT_PASSWORD: my_secret_password
      MYSQL_USER: db_user
      MYSQL_PASSWORD: db_user_pass
      MYSQL_DATABASE: app_db
    ports:
      - "3306:3306"
    volumes:
      - dbdata:/var/lib/mysql
    restart: unless-stopped
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: wavelog-phpmyadmin
    links:
      - db
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
      PMA_ARBITRARY: 1
      PMA_USER: db_user
      PMA_PASSWORD: db_user_pass
    restart: unless-stopped
    ports:
      - 8083:80
volumes:
  dbdata:
```

Note: on some architectures (such as Ampere A1), you may meed to use `:latest` as tag for each image

4. Configure:
    - `MYSQL_USER` and `PMA_USER` with a custom username
    - `MYSQL_PASSWORD` and `PMA_PASSWORD` with a strong password
    - `MYSQL_DATABASE` with any name
5. Please make a note of the parameters you just configured, you will need this value when configuring Wavelog installation
6. Save the file (if you used _nano_ as editor please type `CTRL+O` followed by `CTRL+X`)
7. Execute the stack with `sudo docker-compose up --build -d wavelog-tools`
8. Wait for a confirmation message, if everything was fine you should be able to access `http://address:8083` and access the PhpMyAdmin interface

# Execute Wavelog
Docker containers can be configured in multiple ways, here are three different options:
1. Building a local image
2. Use Portainer
3. Using a prebuilt image
4. Using a standalone stack (recommended for beginners)

## Option 1: Building an image using the provided scripts
### Build the Wavelog image
1. Clone the Wavelog repository
2. Move to the root of the cloned folder
3. Build the image with `sudo docker build -f ./docker/Dockerfile -t wavelog:latest .`

### Launch the Wavelog container
1. Make sure you are in the `docker` folder
2. Execute the stack with `sudo docker-compose up --build -d wavelog`
3. Wait for confirmation and then access the interface at `http://address:8086` to complete the setup

## Option 2: Using Portainer
- Full procedure using Portainer is also described here [IU2FRL/WavelogDocker](https://www.iu2frl.it/wavelog-su-docker-container/)

## Option 3: Prebuilt images
A prebuilt image has been published on Docker Hub at [xxxxxx/wavelog](https://hub.docker.com/r/xxxxxx/wavelog) to be pulled and executed on both `arm64` and `amd64` platforms

## Option 4: Standalone stack (recommended for beginners)

This procedure applies to any setup, either plain Linux, Windows or Portainer, the Docker compose file can be found [here](https://github.com/wavelog/Wavelog/wiki/Executing-the-full-standalone-Wavelog-stack)

# Updating the image
To pull new changes you will need to stop the existing instance, rebuild the Wavelog image and then execute it again.