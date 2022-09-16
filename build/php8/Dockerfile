FROM php:8.1.10-fpm-buster
WORKDIR /tmp
COPY src/sources.list .
COPY src ./src
WORKDIR /tmp/src
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
&& mv /tmp/src/sources.list /etc/apt/sources.list \
&& apt update \
### 默认扩展（docke镜像提供）
&& apt install -y unixodbc-dev freetds-dev libaio-dev firebird-dev libxslt-dev libsodium-dev libtidy-dev libpq-dev libsnmp-dev libpspell-dev  libsqlite3-dev libonig-dev libldb-dev libldap2-dev libkrb5-dev libc-client2007e-dev libenchant-dev libcurl4-openssl-dev net-tools psmisc procps strace libevent-dev libssl-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev libbz2-dev libgmp-dev libffi-dev libzip-dev libxml2-dev unzip \
&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
&& docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr \
&& docker-php-ext-configure pdo_dblib --with-libdir=lib/x86_64-linux-gnu \
&& docker-php-ext-install -j 16 pdo_dblib pdo_odbc pdo_firebird zend_test xsl tidy sodium snmp pspell shmop posix phar pgsql pdo_sqlite pdo_pgsql pdo mbstring ldap imap ftp filter fileinfo enchant dom dl_test dba curl ctype sysvmsg sysvsem sysvshm bcmath bz2 calendar exif ffi gd gettext gmp intl mysqli opcache pcntl pdo_mysql soap sockets zip \
&& docker-php-source extract \
&& cd /usr/src/php/ext/odbc \
&& phpize \
&& sed -ri 's@^ *test +"\$PHP_.*" *= *"no" *&& *PHP_.*=yes *$@#&@g' configure \
&& ./configure --with-unixODBC=shared,/usr \
&& docker-php-ext-install odbc \
&& docker-php-source delete \
&& cd /tmp/src \
### redis
&& pecl install redis-5.3.4.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini \
### memcache
&& pecl install memcache-8.0.tgz && echo "extension=memcache.so" > /usr/local/etc/php/conf.d/memcache.ini \
### composer
&& mv /tmp/src/composer /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
WORKDIR /web


