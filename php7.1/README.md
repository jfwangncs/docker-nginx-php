# docker-nginx-php
docker,nginx+php(mysql,redis,mongodb)

# Build

```
$ docker build -t jfwangncs/nginx-php7.1:1.0 .
```

# Create container

```
$docker run -d -p 80:80 --name v1   jfwangncs/nginx-php7.1:1.0
```
# Logs

```
$ docker logs -f v1
```

## Add volumen
```
$docker run -d -p 80:80 --name v1 -v /var/webapp/test:/webapp  jfwangncs/nginx-php7.1:1.1

```

