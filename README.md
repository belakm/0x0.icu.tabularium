# 0x0.icu.tabularium
Postgresql DB setup for 0x0.icu stack

Includes PostgreSQL and pgAdmin4

# Environment

RancherOS
  |
  -- dockerized nginx (routing)
  -- rancher-server & rancher-agent (spining DB and apis)
  -- certbot (https cert autorenewal)

# Usage

1. Switch to alpine console

```sudo ros console switch alpine```

2. add `nginx.conf` file in `/etc` and  (replace example.com with your domain)

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    root /var/www/html;
    server_name example.com www.example.com;
}
```

2. Run nginx container with this config

```
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /etc/nginx.conf:/etc/nginx/nginx.conf \
  nginx:latest
```

3. Install certbot

3.1. First create a folder `~/certs`

3.2. Run certbot with

```
docker run --rm -it -v ~/certs:/etc/letsencrypt -p 443:443 certbot/certbot certonly --authenticator standalone
```

----

0. alias git as `alias git="docker run -ti --rm -v $(pwd):/git bwits/docker-git-alpine"`

1. clone repo to your to RancherOS

2. create a secrets file in the root and fill in the fields from secrets_example file

3. spin it up in rancher-compose with `rancher-compose --env-file secrets up -d`