FROM php:7.4-fpm-alpine3.16
#COPY ./uploads.ini /etc/php7/conf.d/php-ini-overrides.ini



RUN apk update; \
    apk upgrade;

RUN apk add build-base
RUN apk add bash
RUN apk add curl

RUN docker-php-ext-install mysqli pdo pdo_mysql exif
RUN apk add --no-cache \
        libjpeg-turbo-dev \
        libpng-dev \
        libwebp-dev \
        freetype-dev \
        zlib-dev \
        git

RUN docker-php-ext-enable exif

RUN apk update \
    && apk add autoconf \
    && docker-php-ext-install pcntl

RUN set -xe \
&& apk update \
&& apk upgrade \
&& apk add --no-cache --virtual .phpize-deps $PHPIZE_DEPS \
&& wget https://github.com/FriendsOfPHP/pickle/releases/latest/download/pickle.phar \
&& chmod +x pickle.phar \
&& mv pickle.phar /usr/local/bin/pickle \
&& pickle install -n igbinary \
&& docker-php-ext-enable igbinary \
&& echo "--enable-redis-igbinary" > cfg-options.txt \
&& pickle install --with-configure-options cfg-options.txt redis \
&& docker-php-ext-enable redis \
&& apk del .phpize-deps \
&& rm -rf cfg-options.txt \
&& php -m | grep igbinary \
&& php -m | grep redis \
&& php -i | grep igbinary \
&& php -i | grep redis
#&& CLI_VERSION=$(wp cli info | grep "WP-CLI version" | cut -d$'\t' -f2) \
#&& if [ "${CLI_VERSION}" = "2.4.0" ]; then wp cli update --nightly --yes; fi


# As of PHP 7.4 we don't need to add --with-png
#RUN docker-php-ext-configure gd --with-jpeg --with-webp --with-freetype
RUN docker-php-ext-install gd

# INSTALL COMPOSER
RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer
RUN apk update
RUN apk upgrade
RUN apk add bash
RUN alias composer='php /usr/bin/composer'

#COPY . /var/www/html
WORKDIR /var/www/html





