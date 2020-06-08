# 0x0.icu.via

Reverse proxy setup.

## Environment

```
RancherOS
  |
  |--- dockerized nginx on a network
  |--- pgadmin4
  |--- postgres
  .
  .
  .
  |--- your other services on the network
```

## Usage

### 0. make sure you are running a docker network

```shell
docker network create --driver bridge nginx-net
```

### 1. create volumes for postgres and pgadmin 

```shell
$ mkdir -p /home/rancher/pg/pgdata
$ mkdir -p /home/rancher/pg/admin4
```

### 2. create postgres and pgAdmin env files

```shell
$ cat << EOF > /home/rancher/pg/pg.env
POSTGRES_MODE=primary
POSTGRES_PRIMARY_USER=admin_username
POSTGRES_PRIMARY_PASSWORD=admin_password
POSTGRES_DATABASE=database_name
POSTGRES_USER=database_user
POSTGRES_PASSWORD=database_user_password
POSTGRES_ROOT_PASSWORD=root_password
POSTGRES_PRIMARY_PORT=5432
EOF
```

```shell
$ cat << EOF > /home/rancher/pg/pgadmin2.env
PGADMIN_DEFAULT_EMAIL=pgadmin_email
PGADMIN_DEFAULT_PASSWORD=pgadmin_password
SERVER_PORT=5050
EOF
```

### 3. lax pgadmin4 folder permissions for pgadmin4 docker container

```shell
chmod 777 /home/rancher/pg/pgadmin4
```

### 4. run dockerized postgres and pgadmin

```shell
$ docker run -d \
  --name postgres \
  --expose 5432 \
  --restart unless-stopped \
  --volume=/home/rancher/pg/postgres:/var/lib/postgresql/data \
  --env-file=/home/rancher/pg/pg.env \
  --name="postgres" \
  --hostname="postgres" \
  --network="nginx-net" \
  postgres:alpine
```

```shell
$ docker run -d \
  --name pgadmin \
  --expose 5050 \
  --restart unless-stopped \
  --volume=/home/rancher/pg/pgadmin4:/var/lib/pgadmin \
  --env-file=/home/rancher/pg/pgadmin.env \
  --name="pgadmin4" \
  --hostname="pgadmin4" \
  --network="nginx-net" \
  dpage/pgadmin4
```

### 5. setup nginx to work with pgadmin

Example configuration for /pgadmin4 route

```nginx
# ... server block

location /pgadmin4/ {
  proxy_set_header Host $host;
  proxy_set_header X-Script-Name /pgadmin4;
  proxy_pass http://pgadmin4/;
  proxy_redirect off;
}
```