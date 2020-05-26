# 0x0.icu.tabularium
Postgresql DB setup for 0x0.icu stack

Includes PostgreSQL and pgAdmin4

# Environment

RancherOS

# Usage

0. clone to RancherOS

1. create a secrets file in the root and fill in the fields from secrets_example file

2. spin it up in rancher-compose with `$ rancher-compose --env-file secrets up -d`