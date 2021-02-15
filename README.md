## Docker environment

- Redis
- MariaDB
- PHP-FPM (7.4.12)
- NGINX

## Usage

build & start
```
$ docker-compose up -d --build
```

remove
```
$ docker-compose down
```

remove everything (volumes, images, etc.)
```
$ docker-compose down --rmi all -v --remove-orphans
```