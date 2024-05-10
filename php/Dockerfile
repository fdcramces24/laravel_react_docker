#=========================================
# Multi stage build - base image
#=========================================
FROM php:8.3-fpm-bullseye AS base

# Work dir definition
ENV WORKDIR=/var/www/
WORKDIR $WORKDIR

# timezone environment
ENV TZ=UTC \
    # locale
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US:en \
    LC_ALL=en_US.UTF-8 \
    # composer environment
    COMPOSER_ALLOW_SUPERUSER=1 \
    COMPOSER_HOME=/composer

COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

RUN <<EOF
    apt-get update && \
    apt-get -y install --no-install-recommends \
        locales \
        git \
        unzip \
        libzip-dev \
        libicu-dev \
        libonig-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libwebp-dev \
        libxpm-dev \
        vim \
        mecab \
        libmecab-dev \
        mecab-utils \
        mecab-jumandic-utf8 \
        mecab-naist-jdic \
        python3-mecab \
        sudo \
        && \
    locale-gen en_US.UTF-8 && \
    localedef -f UTF-8 -i en_US en_US.UTF-8 && \
    docker-php-ext-configure gd \
        --with-jpeg \
        --with-freetype \
        --with-webp \
        --with-xpm \
        && \
    docker-php-ext-install \
        intl \
        pdo_mysql \
        zip \
        bcmath \
        exif \
        pcntl \
        sockets \
        gd && \
    mkdir -p /tmp/pear/cache && \
    pecl channel-update pecl.php.net && \
    pecl clear-cache && \
    pecl install redis-5.3.4 && \
    docker-php-ext-enable redis && \
	docker-php-ext-install opcache && \
    composer config -g process-timeout 3600 && \
    composer config -g repos.packagist composer https://packagist.org
EOF

# Place php.ini file into the image
COPY ./laravel_react_docker/php/php.ini /usr/local/etc/php/php.ini
