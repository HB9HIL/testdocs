# Docker-Support

We're proud to provide Docker-Support now.
You can find the latest master-Image at "Packages".

## How to install/Use?

1. Create a docker-compose-file like the one at the bottom here.
2. Adjust the exposed port (if you like, default is 8086) and change database password (but remember them!)
3. Run the docker compose with `docker compose up -d`.
4. Wavelog is now online at http://[docker-machine]:[exposed-port - default 8086]/
5. For first time installation follow the installer. Hostname for the DB is "**wavelog-db**" (unless changed). Credentials see yaml
6. Enjoy using Wavelog

### Updating

1. Stop your Stack/Container

```bash
docker compose down
```
2. Repull the image

```bash
docker compose pull
```

3. Start it

```bash
docker compose up -d
```

> [!TIP]
> Did you know? Adding [Watchtower](https://containrrr.dev/watchtower/) to your stack keeps Wavelog automatically updated!

### Tweaking `config.php` for QRZ.com access

1. Edit the config.php (located in the volume `wavelog-config`)

### On-Top-Informations

#### Debugging

1. Basic logging is available with: `docker logs --follow [container-id]` (Obtain container-id via your UI or `docker ps -a`)
2. raise `loglevel` in the file `config.php` (see **tweaking config.php** above) and tail the log (you can get a shell access to the container with `docker exec -it [container-id] bash`) - logs are located at `./application/logs`

#### Cronjobs

In the Docker version of Wavelog is the Cronmanager enabled per default. You can manage the cronjobs in de WebUI via Admin > Cronmanager

### Questions?

Check out our [discussions area](https://github.com/wavelog/wavelog/discussions)! .

---

### docker-compose.yaml

```yaml
services:
  wavelog-db:
    image: mariadb:11.3
    container_name: wavelog-db
    environment:
      MARIADB_RANDOM_ROOT_PASSWORD: yes
      MARIADB_DATABASE: wavelog
      MARIADB_USER: wavelog
      MARIADB_PASSWORD: wavelog # <- Insert a strong password here
    volumes:
      - wavelog-dbdata:/var/lib/mysql
    restart: unless-stopped

  wavelog-main:
    container_name: wavelog-main
    image: ghcr.io/wavelog/wavelog:latest
    depends_on:
      - wavelog-db
    environment:
      CI_ENV: docker
    volumes:
      - wavelog-config:/var/www/html/application/config/docker
      - wavelog-uploads:/var/www/html/uploads
      - wavelog-userdata:/var/www/html/userdata
    ports:
      - "8086:80"
    restart: unless-stopped

volumes:
  wavelog-dbdata:
  wavelog-uploads:
  wavelog-userdata:
  wavelog-config:
```

## Reverse proxy

If you run an application in a private container network, you may need to proxy traffic to it using a reverse proxy or a DNAT rule. Since a reverse proxy is more useful if you have multiple services, here is an example nginx config to reverse proxy:

```
server {
    listen  *:80;
    server_name  wavelog.example.com;
    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        # this needs to be the location how nginx can access your container - either hostname or IP address
        proxy_pass http://wavelog-container.example.com:8086;
    }

    # nginx still need to serve error pages - for example if your container is down
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

It is recommended to support https. See [webserver configuration](Webserver-Configurations) for a more detailed example config.

## Database connection

Once you access your Wavelog installation, you will have to provide information to the database within the install wizard.
As per the docker compose file, your database name is 'wavelog', your user and password are 'wavelog' as well (Make sure to change at least the password). Your hostname for the database is 'wavelog-db', just as the service name. If you change the service name, so will the host of the database.
