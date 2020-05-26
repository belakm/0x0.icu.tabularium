# 0x0.icu.tabularium
Postgresql DB setup for 0x0.icu stack

Includes PostgreSQL and pgAdmin4

# Environment

RancherOS

# Usage

0. alias git as `alias git="docker run -ti --rm -v $(pwd):/git bwits/docker-git-alpine"`

1. clone repo to your to RancherOS

2. create a secrets file in the root and fill in the fields from secrets_example file

3. spin it up in rancher-compose with `$ rancher-compose --env-file secrets up -d`