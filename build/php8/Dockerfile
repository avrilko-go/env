FROM php:8.1.10-fpm-buster
WORKDIR /tmp
COPY src/sources.list .
COPY src ./src
WORKDIR /tmp/src
ENV LD_LIBRARY_PATH /opt/oracle/instantclient/
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
&& mv /tmp/src/sources.list /etc/apt/sources.list \
&& apt update \
### 默认扩展（docke镜像提供）
&& apt install -y libmcrypt-dev libmagickwand-dev libmemcached-dev unixodbc-dev freetds-dev libaio-dev firebird-dev libxslt-dev libsodium-dev libtidy-dev libpq-dev libsnmp-dev libpspell-dev  libsqlite3-dev libonig-dev libldb-dev libldap2-dev libkrb5-dev libc-client2007e-dev libenchant-dev libcurl4-openssl-dev net-tools psmisc procps strace libevent-dev libssl-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev libbz2-dev libgmp-dev libffi-dev libzip-dev libxml2-dev unzip \
&& mkdir /opt/oracle && mv /tmp/src/instantclient-basic-linux.x64-21.7.0.0.0dbru.zip /opt/oracle/instantclient-basic-linux.x64-21.7.0.0.0dbru.zip && mv /tmp/src/instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip /opt/oracle/instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip && cd /opt/oracle && unzip instantclient-basic-linux.x64-21.7.0.0.0dbru.zip && unzip instantclient-sdk-linux.x64-21.7.0.0.0dbru.zip && mv instantclient_21_7 instantclient &&  ldconfig \
&& docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient,21.7 \
&& docker-php-ext-configure oci8 --with-oci8=instantclient,/opt/oracle/instantclient,21.7 \
&& docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
&& docker-php-ext-configure pdo_odbc --with-pdo-odbc=unixODBC,/usr \
&& docker-php-ext-configure pdo_dblib --with-libdir=lib/x86_64-linux-gnu \
&& docker-php-ext-install -j 16 oci8 pdo_oci pdo_dblib pdo_odbc pdo_firebird zend_test xsl tidy sodium snmp pspell shmop posix phar pgsql pdo_sqlite pdo_pgsql pdo mbstring ldap imap ftp filter fileinfo enchant dom dl_test dba curl ctype sysvmsg sysvsem sysvshm bcmath bz2 calendar exif ffi gd gettext gmp intl mysqli opcache pcntl pdo_mysql soap sockets zip \
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
### memcached
&& pecl install memcached-3.2.0.tgz && echo "extension=memcached.so" > /usr/local/etc/php/conf.d/memcached.ini \
### xdebug
&& pecl install xdebug-3.1.5.tgz && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini \
### imagick
&& pecl install imagick-3.7.0.tgz && echo "extension=imagick.so" > /usr/local/etc/php/conf.d/imagick.ini \
### mcrypt
&& pecl install mcrypt-1.0.5.tgz && echo "extension=mcrypt.so" > /usr/local/etc/php/conf.d/mcrypt.ini \
### composer
&& mv /tmp/src/composer /usr/local/bin/composer && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
WORKDIR /web


