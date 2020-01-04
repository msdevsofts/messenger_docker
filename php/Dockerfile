FROM php:7.3-fpm-alpine

RUN apk upgrade --update && apk --no-cache add \
    icu-dev \
    # xdebug
    autoconf make g++ gcc \
    php7-gd \
    freetype freetype-dev \
    libpng libpng-dev \
    libjpeg jpeg-dev libjpeg-turbo libjpeg-turbo-dev \
    libtool libxml2-dev \
    libxpm libxpm-dev \
    libwebp libwebp-dev \
    imagemagick-dev

RUN docker-php-ext-configure gd \
  --with-freetype-dir=/usr/include/ \
  --with-png-dir=/usr/include/ \
  --with-jpeg-dir=/usr/include/ \
  --with-xpm-dir=/usr/include/ \
  --with-webp-dir=/usr/include/ \
  --with-gd && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install  -j${NPROC} iconv gd exif intl mbstring pdo_mysql && \
  pecl install xdebug-2.7.0beta1 && \
  docker-php-ext-enable xdebug && \
  pecl install imagick && \
  docker-php-ext-enable imagick

WORKDIR /var/www/html/

ADD ./app/ /var/www/html/
RUN chown -R www-data: /var/www/html/