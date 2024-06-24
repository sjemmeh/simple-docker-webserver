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
Edit the docker-compose.yml and config files where neccesary, then run the following:<br />


```
docker compose build && docker compose up -d 
```