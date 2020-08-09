FROM ubuntu:16.04
MAINTAINER "jfwangncs" <475760135@qq.com>
ENV php_conf /etc/php/5.6/fpm/php.ini
RUN     apt-get update \
	&& apt-get install software-properties-common \
	&& add-apt-repository ppa:ondrej/php \
        && apt-get update \
        && ln -s /usr/lib/x86_64-linux-gnu/libssl.so  /usr/lib \
	&& apt-get install --no-install-recommends --no-install-suggests  -y nginx \
									     php5.6-fpm \
									     supervisor \
									     php5.6-dev \
									     wget \
									     build-essential \
									     php5.6-mysql \
									     php5.6-xml \
									     php5.6-curl \
									     php5.6-mbstring \
	&& sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" ${php_conf} \
	&& sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" ${php_conf} \
	&& sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" ${php_conf} \
	&& sed -i -e "s/variables_order = \"GPCS\"/variables_order = \"EGPCS\"/g" ${php_conf} \
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
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir /run/php
copy conf/php-fpm.conf /etc/php/7.0/fpm/php-fpm.conf
copy conf/nginx.conf /etc/nginx/nginx.conf 
copy conf/default /etc/nginx/sites-enabled/default
copy conf/supervisord.conf /etc/supervisord.conf
#Nginx logs to Docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log && \
	ln -sf /dev/stderr /var/log/nginx/error.log
EXPOSE 80 443
CMD ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

