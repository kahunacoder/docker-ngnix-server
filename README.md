# docker-ngnix-server
Configure a ngnix server with varaibles from .env files.

Assumes directory structure as follows
```
app
    -.env
    -public
    -server
        -logs
docker
    -ngnix
        -conf
            server.template
        -certs
            server.crt
            server.key
docker-compose.yml
```


## Docker Compose Config

docker-compose.yml

```
...
nginx:
    image: kahunacoder/docker-ngnix-server
    ports:
        - 80:80
        - 443:443
    links:
        - php
    volumes_from:
        - app
    volumes:
        - ./docker/nginx/conf/:/etc/nginx/conf.d/
        - ./app/server/log/:/var/log/nginx/
    env_file: ./app/.env
    command: /bin/bash -c "envsubst < /etc/nginx/conf.d/server.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"
...
```
