FROM php:7.4-fpm

RUN echo "Launch php"

RUN rm /etc/apt/preferences.d/no-debian-php \
    && apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libmcrypt-dev \
        libmagickwand-dev \
        libxml2-dev \
        zlib1g-dev \
        imagemagick \
        libzip-dev \
        unzip \
        git \
        msmtp \
	&& docker-php-ext-install -j$(nproc) iconv \
	&& docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
	&& docker-php-ext-install -j$(nproc) soap \
	&& docker-php-ext-install -j$(nproc) zip

RUN apt-get install -y zlib1g-dev libicu-dev g++ && docker-php-ext-configure intl
RUN pecl install imagick && docker-php-ext-enable imagick \
    && docker-php-ext-install exif

RUN docker-php-ext-configure calendar && docker-php-ext-install calendar

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --1 --install-dir=/usr/bin/ --filename=composer \
    && php -r "unlink('composer-setup.php');"
WORKDIR /var/www/html
COPY ./php.ini /usr/local/etc/php/

RUN usermod -u 1000 www-data
