# Docker container for nginx, with authentik
Generate .env with the following command:
```
cat >> .env << EOF
# Default akadmin password
BS_PASS=$(openssl rand 60 | base64 -w 0)

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

###################################
# Letsencrypt-DNS configuration file
###################################

# Letsencrypt email address
LETSENCRYPT_EMAIL=<EMAIL HERE>

# Provider options
LETSENCRYPT_PROVIDER_OPTIONS=--auth-username=<USERNAME HERE> --auth-token=<AUTH TOKEN HERE>

EOF
```
Create a domains.conf file in nginx/letsencrypt/domains.conf

Add a line for each domain you want to use letsencrypt for:
```
*.example.com
example.com
```

Edit the .env: [authentik's docs](https://docs.goauthentik.io/docs/installation/docker-compose)

## Place SSL certificate in following folder:
```
nginx/ssl/cert.pem
ngix/ssl/key.pem
```
My personal preference is to use a cloudflare origin CA: [Origin Configuration](https://developers.cloudflare.com/ssl/origin-configuration/)
If you want to use Letsencrypt you can use the [certbot](https://hub.docker.com/r/certbot/certbot) container.

## Edit the docker-compose.yml and config files where neccesary, then run the following:

```
docker compose build && docker compose up -d 
```

# Configure Authentik:
* Navigate to http://***server-ip***:9000/ to configure authentik (default user: akadmin, pass is in .env file under "BS_PASS")
* In the admin panel go to Applications > Outposts, and edit the default outpost:
  * Under advanced settings, set the "authentik_host" variable to your own host (eg auth.example.com)

## Securing a domain with authentik:
* In the admin panel go to Applications > Applications
* On the top press "Create with Wizard"
* In Application Details, under name, enter the name of the application, the slug is should be autofilled
* In Provider Type select "Forward Auth (Single Application)
* Name the application again and select the authorization flow. The external host is the (sub)domain you're trying to secure (eg. https://nextcloud.example.com)
* When you have created the application, go to Applications > Outposts, and edit the outpost. Add the previously added application to the list and press update.

## Creating a (reverse-proxy) that uses authentik:
In the example files:
* Uncomment the authentik-specific areas, if you have changed the docker-compose service name, change to the new value.
