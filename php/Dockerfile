FROM i386/php:8.0

ENV PHP_MONGO_VERSION 1.4.2
ENV LIBRARY_PATH=/lib:/usr/lib
RUN apt-get update 
RUN apt-get -y install \
        autoconf \
        cmake \
        curl \
        file \
        g++ \
        gcc \
        libicu-dev \
        imagemagick \
        libc-dev \
        libjpeg-dev \
        libpng-dev \
        libtool \
        libxml2-dev \
        make \
        libpcre3 libpcre3-dev \
        pkgconf \
        re2c \
        libzip-dev \
        libcurl4-openssl-dev \
        zlib1g 
RUN apt-get -y install \
        git \
        curl \
        bash \
        bash-completion \
        imagemagick \
        libintl-perl \
        libltdl-dev \
        libonig-dev \
        wget 
RUN docker-php-ext-configure gd --enable-gd --with-jpeg && \
    docker-php-ext-configure bcmath && \
    docker-php-ext-install \
        soap \
        zip \
        curl \
        bcmath \
        exif \
        gd \
        iconv \
        intl \
        mbstring \
        opcache 
# Configure version constraints
ENV PHP_ENABLE_XDEBUG=1 \
    PATH=/app:/app/vendor/bin:/root/.composer/vendor/bin:$PATH \
    TERM=linux \
    COMPOSER_ALLOW_SUPERUSER=1
RUN apt-get -y install libmagick++-dev

RUN pecl install  imagick && \
    echo "extension=imagick.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
RUN pecl install mongodb && \
    echo "extension=mongodb.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`
RUN apt-get -y install libmcrypt++-dev

RUN pecl install mcrypt && \
    echo "extension=mcrypt.so" >> `php --ini | grep "Loaded Configuration" | sed -e "s|.*:\s*||"`

RUN apt-get update && apt-get install -y dos2unix
RUN dos2unix /usr/local/bin/docker-php-entrypoint && apt-get --purge remove -y dos2unix

# Add configuration files
COPY image-files/ /

# Add GITHUB_API_TOKEN support for composer
RUN chmod 700 \
        /usr/local/bin/docker-php-entrypoint \
        /usr/local/bin/composer

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer.phar \
        --install-dir=/usr/local/bin && \
    composer clear-cache

# Application environment
WORKDIR /app

CMD ["php-fpm"]