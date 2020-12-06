FROM php:8.0.0-fpm-alpine

RUN apk upgrade --update && apk --no-cache add \
    icu-dev \
    autoconf make g++ gcc git nodejs npm \
    zip libzip-dev \
    freetype freetype-dev \
    libpng libpng-dev \
    libjpeg jpeg-dev libjpeg-turbo libjpeg-turbo-dev \
    libtool libxml2-dev \
    libxpm libxpm-dev \
    libwebp libwebp-dev \
    imagemagick-dev \
    oniguruma-dev

RUN docker-php-ext-configure gd \
  --with-freetype=/usr/include/ \
  --with-jpeg=/usr/include/ \
  --with-xpm=/usr/include/ \
  --with-webp=/usr/include/ && \
  NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
  docker-php-ext-install  -j${NPROC} iconv gd exif intl pdo_mysql zip && \
  docker-php-ext-enable sodium && \
  pecl install xdebug && \
  docker-php-ext-enable xdebug

RUN git clone https://github.com/Imagick/imagick && \
  cd imagick && \
  sed -i "s/#define PHP_IMAGICK_VERSION .*/#define PHP_IMAGICK_VERSION \"git-master-$(git rev-parse --short HEAD)\"/" php_imagick.h && \
  phpize && \
  ./configure && \
  make && \
  make install && \
  docker-php-ext-enable imagick && \
  cd .. && \
  rm -r imagick

COPY --from=composer /usr/bin/composer /usr/bin/composer
ENV PATH $PATH:/root/.composer/vendor/bin/
RUN composer global require "laravel/installer"
RUN composer global require "composer require laravelcollective/html"

WORKDIR /var/www/html/

ADD ./app/ /var/www/html/
RUN chown -R www-data: /var/www/html/

RUN composer install
RUN npm install && npm run dev && npm audit fix
