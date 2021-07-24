FROM php:8-fpm
WORKDIR /tmp
COPY src/sources.list .
COPY src ./src
COPY src/fiber /usr/src/php/ext/fiber/
COPY src/event /usr/src/php/ext/event/
WORKDIR /tmp/src
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
&& mv /tmp/src/sources.list /etc/apt/sources.list \
&& apt-get update \

### 默认扩展（docke镜像提供）
&& apt-get install -y net-tools psmisc procps strace libevent-dev libssl-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev libbz2-dev libgmp-dev libffi-dev libzip-dev libxml2-dev unzip \
&& docker-php-ext-install sysvmsg sysvsem sysvshm bcmath bz2 calendar exif ffi gd gettext gmp intl mysqli opcache pcntl pdo_mysql soap sockets zip \
&& cd /usr/src/php/ext/event && docker-php-source extract && docker-php-ext-install --ini-name zevent.ini event \
&& cd /usr/src/php/ext/fiber && docker-php-source extract && docker-php-ext-install fiber \
&& cd /tmp/src \
### redis
&& pecl install redis-5.3.4.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini \
### xdebugs
&& pecl install xdebug-3.0.4.tgz && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini \
### memcache
&& pecl install memcache-8.0.tgz && echo "extension=memcache.so" > /usr/local/etc/php/conf.d/memcache.ini \
### composer
&& mv /tmp/src/composer /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
WORKDIR /web


