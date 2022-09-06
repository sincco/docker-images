FROM php:7.4-apache

RUN apt-get update &&\
    apt-get install --no-install-recommends --assume-yes --quiet ca-certificates curl git &&\
    rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && DEBIAN_FRONTEND=noninteractive apt-get install --assume-yes --no-install-recommends \
    nano \
    wget \
    cron \
    zip unzip \
    zlib1g-dev libpng-dev libjpeg-dev libzip-dev libfreetype6-dev libmcrypt-dev \
    bash-completion \
    && apt-get clean

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer 

RUN docker-php-ext-configure gd --with-jpeg --with-freetype

RUN docker-php-ext-install mysqli pdo pdo_mysql gd zip

RUN a2enmod rewrite; a2enmod headers; a2enmod ssl

RUN ln -sf /dev/stdout /var/log/apache2/access.log \
    && ln -sf /dev/stderr /var/log/apache2/error.log

RUN pecl install xdebug-2.9.8 && docker-php-ext-enable xdebug
RUN echo 'zend_extension="/usr/local/lib/php/extensions/no-debug-non-zts-20190902/xdebug.so"' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_port=9000' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_enable=1' >> /usr/local/etc/php/php.ini
RUN echo 'xdebug.remote_connect_back=1' >> /usr/local/etc/php/php.ini
RUN echo 'upload_max_filesize = 20M' >> /usr/local/etc/php/php.ini
RUN echo 'post_max_size = 21M' >> /usr/local/etc/php/php.ini

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN usermod -u 1000 www-data
