FROM php:${PHP_VERSION:-7.1-fpm}

RUN apt-get update && apt-get install -y \
    git \
    unzip \
    zlib1g-dev \
    zlib1g \
    libicu-dev \
    g++ \
    curl \
    libcurl4-gnutls-dev \
    libxml2-dev \
    libpng12-dev \
    libc-client-dev \
    libkrb5-dev \
    apt-utils \
    libfontconfig1 \
    libxrender1 \
    libmcrypt-dev \
    openssl

RUN echo "deb http://www.deb-multimedia.org jessie main non-free" >>/etc/apt/sources.list
RUN echo "deb-src http://www.deb-multimedia.org jessie main non-free" >>/etc/apt/sources.list
RUN apt-get update && apt-get install -y --force-yes deb-multimedia-keyring

RUN apt-get update && apt-get install -y imagemagick inkscape pngnq pngcrush xvfb cabextract \
        poppler-utils xz-utils libreoffice libreoffice-math jpegoptim ffmpeg \
        html2text ghostscript exiftool

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN apt-get install -y xfonts-75dpi
RUN apt-get install -y wkhtmltopdf

RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap

RUN docker-php-ext-install pdo pdo_mysql zip mbstring intl curl xml soap bcmath iconv mcrypt dom simplexml exif

RUN apt-get install -y libpq-dev libmemcached-dev
RUN pecl install memcached
RUN docker-php-ext-enable memcached

RUN docker-php-ext-configure opcache --enable-opcache \
&& docker-php-ext-install opcache

RUN pecl install redis
RUN docker-php-ext-enable redis

RUN apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng12-dev \
    && docker-php-ext-configure gd \
          --enable-gd-native-ttf \
          --with-freetype-dir=/usr/include/freetype2 \
          --with-png-dir=/usr/include \
          --with-jpeg-dir=/usr/include \
    && docker-php-ext-install gd \
    && docker-php-ext-enable gd

RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN apt-get install -y libmagickwand-dev --no-install-recommends && rm -rf /var/lib/apt/lists/*
RUN pecl install imagick && docker-php-ext-enable imagick

RUN apt-get update && apt-get install -y libbz2-dev
RUN docker-php-ext-install bz2

RUN docker-php-ext-install mysqli