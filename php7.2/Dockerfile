FROM ubuntu:18.04
MAINTAINER "jfwangncs" <475760135@qq.com>
ENV php_conf /etc/php/7.2/fpm/php.ini
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
	&& apt-get install -y language-pack-en-base \
	&& locale-gen en_US.UTF-8 \		
	&& apt-get install -y software-properties-common \
	&& LC_ALL=en_US.UTF-8 apt-add-repository  ppa:ondrej/php \
	&& apt-get update \
	&& ln -s /usr/lib/x86_64-linux-gnu/libssl.so  /usr/lib \
	&& apt-get install --no-install-recommends --no-install-suggests  -y nginx \								     
										 php7.2-fpm \
									     supervisor \
									     php7.2-dev \
									     wget \
									     build-essential \
									     php7.2-mysql \
									     php7.2-xml \
									     php7.2-curl \
									     php7.2-mbstring \
										 php7.2-zip \
										 php7.2-gd \
	&& sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" ${php_conf} \
	&& sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" ${php_conf} \
	&& sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" ${php_conf} \
	&& sed -i -e "s/variables_order = \"GPCS\"/variables_order = \"EGPCS\"/g" ${php_conf} \
	#node
    && cd /usr/local/src \
	&& wget https://nodejs.org/dist/v12.16.1/node-v12.16.1.tar.gz \
	&& tar xf node-v12.16.1.tar.gz \
	&& cd node-v12.16.1 \
	$$ chmod -R 755 node-v12.16.1 \
	&& ./configure \
	&& make \
	&& make install \
	&& npm config set registry https://registry.npm.taobao.org \
	$$ rm /usr/local/src/node-v12/16.1.tar.gz \
	$$ rm -r /usr/local/src/node-v12.16.1 \
	#redis
	&& cd /usr/local/src \
	&& wget http://pecl.php.net/get/redis-4.0.2.tgz \
	&& tar zxvf redis-4.0.2.tgz \
	&& cd redis-4.0.2 \
	&& phpize \
	&& ./configure \
	&& make \
	&& make install \
	&& echo "extension=redis.so;" >> ${php_conf} \
	&& rm /usr/local/src/redis-4.0.2.tgz \
	&& rm -r /usr/local/src/redis-4.0.2 \	
	#mongodb
	&& cd /usr/local/src \
	&& wget http://pecl.php.net/get/mongodb-1.4.4.tgz \
	&& tar zxvf mongodb-1.4.4.tgz \
	&& cd mongodb-1.4.4 \
	&& phpize \
	&& ./configure \
    && make \
	&& make install \
	&& echo "extension=mongodb.so;" >> ${php_conf} \
	&& rm /usr/local/src/mongodb-1.4.4.tgz \
    && rm -r /usr/local/src/mongodb-1.4.4 \
	&& rm -rf /var/lib/apt/lists/*
copy conf/php-fpm.conf /etc/php/7.2/fpm/php-fpm.conf
copy conf/nginx.conf /etc/nginx/nginx.conf 
copy conf/default /etc/nginx/sites-enabled/default
copy conf/supervisord.conf /etc/supervisord.conf
#Nginx logs to Docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
	ln -sf /dev/stderr /var/log/nginx/error.log
EXPOSE 80 443
CMD ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

