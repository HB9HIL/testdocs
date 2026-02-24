# Please read carefully
**⚠️ Warning**: It is your responsibility to ensure you protect your system from intruders/attacks. These commands and permissions are just examples used to get Wavelog up and running and are not a guide on how to achieve a secure system. You should review these permissions after installation and make appropriate changes if you determine that finer-grained access control is needed.

**⚠️ Warning**: Docker support is highly experimental, you may run into different issues when updating your existing setup, manual intervention may be needed to align tables and/or config files to the new version.

# Instructions

This Docker Compose script makes sure all the services are ready to run the Wavelog installation. Persistency is enabled by default on both MySQL and Wavelog

## Step 1: Create `docker-compose.yml`

1. Connect to the machine where you want to deploy Wavelog
2. Create a new `docker-compose.yml`
3. Paste the following code

```yaml
version: '3'
services:
  wavelog-mysql:
    image: mysql:latest
    container_name: wavelog-mysql
    environment:
      MYSQL_RANDOM_ROOT_PASSWORD: yes
      MYSQL_DATABASE: wavelog
      MYSQL_USER: wavelog
      MYSQL_PASSWORD: wavelog-pass # <- Insert a strong password here
    #ports:
      #- "3306:3306" # <- Uncomment these lines only if you need to expose the MySQL container to the network
    volumes:
      - wavelog-dbdata:/var/lib/mysql
    restart: unless-stopped

  wavelog-phpmyadmin:
    image: phpmyadmin:latest
    container_name: wavelog-phpmyadmin
    depends_on:
      - wavelog-mysql
    environment:
      PMA_HOST: wavelog-mysql
      PMA_PORT: 3306
      PMA_USER: wavelog
      PMA_PASSWORD: wavelog-pass # <- Put same strong password here
    #restart: unless-stopped
    ports:
      - "8083:80"
      
  wavelog-main:
    image: xxxxxx/wavelog:latest
    container_name: wavelog-main
    depends_on:
      - wavelog-mysql
    volumes:
      - wavelog-config:/var/www/html/application/config
      - wavelog-backup:/var/www/html/application/backup
      - wavelog-uploads:/var/www/html/application/uploads
    ports:
      - "8086:80"
    restart: unless-stopped

volumes:
  wavelog-dbdata:
  wavelog-config:
  wavelog-backup:
  wavelog-uploads:
```

### Important notes

- Make sure to provide a strong password in the `docker-compose.yml`
- PhpMyAdmin should be stopped after the first setup and executed only for debugging purposes
- MySQL is only accessible from the Docker environment, if you want to expose it please uncomment the `ports:` lines (not recommended)

## Step 2: Execute the stack

1. Execute `docker compose up -d`

You should see the terminal downloading some stuff and then returning to the normal blinking cursor, if no errors are shown you are good to go!

## Step 3: Configure Wavelog

1. Access Wavelog at `http://ipaddress:8086/install`
2. Set:
    - Directory: _leave blank_
    - Website: _should be set already_
    - Default gridsquare: your QTH locator
    - DB Hostname: `wavelog-mysql`
    - DB User: `wavelog`
    - DB Pass: the password you entered in step 1
    - DB Name: `wavelog`
3. Click on `Install`

## Step 4: Enjoy!

# Upgrading

When you need to upgrade your Wavelog installation, it is just needed to stop the stack, pull the new image (which is usually tagged as `latest`) and run the stack again.

## Perform a DB backup

1. Make sure the PhpMyAdmin image is running
2. Open `http://ipaddress:8083`
3. Go to the `Export` tab and click the `Export` button
4. Store the SQL file in a safe place

## Pull latest image

### Using Portainer

I you are using Portainer, you can simply go to `Stack`, select the Wavelog stack, click on `Editor` and then `Deploy` without touching anything, a dialog should appear asking you if you want to pull the latest images, make sure to enable such option

### Using command line

First we need to move to the folder where we created the `docker-compose.yml` file before

```bash
cd /path/to/docker-compose.yml
docker-compose stop
docker-compose rm -f
docker-compose pull   
docker-compose up -d
```

## Configuration after upgrade

A configuration is needed after the upgrade, open your browser to `http://ipaddress:8086/install` and insert the same data you set the first time, everything should be restored automatically. 

### Restore configuration

If something got lost during the upgrade procedure, open the PhpMyAdmin interface and restore the SQL backup