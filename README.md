## Simple docker container for webserver
Create a .env file that contains the following:<br />
```
PUID=
PGID=
MYSQL_ROOT_PASSWORD=
MYSQL_DATABASE=
MYSQL_USER=
MYSQL_PASSWORD=
```

Place SSL certificate in following folder:
```
nginx/ssl/cert.pem
ngix/ssl/key.pem
```
If you want to use Letsencrypt you can use the [certbot](https://hub.docker.com/r/certbot/certbot) container and edit the config to your own liking

Edit the docker-compose.yml and config files where neccesary, then run the following:<br />


```
docker compose build && docker compose up -d 
```