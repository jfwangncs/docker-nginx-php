# docker-nginx-php
docker,nginx+php(mysql,redis,mongodb)

# Build

```
$ docker build -t nginx-php7.0 .
```

# Create container

```
$docker run -d -p 80:80 --name v1   jfwangncs/nginx-php7.0
```
# Logs

```
$ docker logs -f webserver01#Add volumen
```

## Add volumen
```
$docker run -d -p 80:80 --name v1 -v /var/webapp/test:/webapp  jfwangncs/nginx-php7.0
```

