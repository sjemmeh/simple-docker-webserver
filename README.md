## Simple docker container for webserver, with authentik
Generate .env with the following command:
```
cat >> .env << EOF
PG_PASS=$(openssl rand 36 | base64 -w 0)
AUTHENTIK_SECRET_KEY=$(openssl rand 60 | base64 -w 0)
AUTHENTIK_BOOTSTRAP_PASSWORD=$(openssl rand 60 | base64 -w 0)

# SMTP Host Emails are sent to
AUTHENTIK_EMAIL__HOST=localhost
AUTHENTIK_EMAIL__PORT=25
# Optionally authenticate (don't add quotation marks to your password)
AUTHENTIK_EMAIL__USERNAME=
AUTHENTIK_EMAIL__PASSWORD=
# Use StartTLS
AUTHENTIK_EMAIL__USE_TLS=false
# Use SSL
AUTHENTIK_EMAIL__USE_SSL=false
AUTHENTIK_EMAIL__TIMEOUT=10
# Email address authentik will send from, should have a correct @domain
AUTHENTIK_EMAIL__FROM=authentik@localhost
EOF
```

Edit the .env: [authentik's docs](https://docs.goauthentik.io/docs/installation/docker-compose)

Place SSL certificate in following folder:
```
nginx/ssl/cert.pem
ngix/ssl/key.pem
```
If you want to use Letsencrypt you can use the [certbot](https://hub.docker.com/r/certbot/certbot) container and edit the config to your own liking

Edit the docker-compose.yml and config files where neccesary, then run the following:


```
docker compose build && docker compose up -d 
```

Then navigate to http://***server-ip***:9000/ to configure authentik (default user: akadmin, pass is in .env file under "BS_PASS")
