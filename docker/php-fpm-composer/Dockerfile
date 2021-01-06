FROM php:7.3.6-fpm

RUN apt-get update && apt-get install -y --fix-missing \
    mysql-client \
    imagemagick \
    graphviz \
    git \
    libpng-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libxml2-dev \
    libxslt1-dev \
    wget \
    linux-libc-dev \
    libyaml-dev \
    libzip-dev \
    libicu-dev \
    libpq-dev \
    libssl-dev && \
    rm -r /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install \
    mysqli \
    pdo_mysql \
    gd \
    mbstring \
    xsl \
    opcache \
    calendar \
    intl \
    exif \
    ftp \
    bcmath \
    zip
RUN apt-get update && apt-get install -y libpq-dev && docker-php-ext-install pdo pdo_pgsql
ADD xdebug.ini  /etc/php7.3/conf.d/

RUN echo "upload_max_filesize = 500M\n" \
         "post_max_size = 500M\n" \
         > /usr/local/etc/php/conf.d/maxsize.ini

# composer install
ENV COMPOSER_HOME /composer
ENV PATH /composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1

COPY --from=composer /usr/bin/composer /usr/bin/composer
# git, zip & unzip tools
RUN apt-get update -y && apt-get install -y git zip unzip && rm -rf /var/lib/apt/lists/*

RUN mkdir -p "$COMPOSER_HOME/cache" \
&& mkdir -p "$COMPOSER_HOME/vendor" \
&& chown -R www-data:www-data $COMPOSER_HOME \
&& chown -R www-data:www-data /var/www


USER www-data

VOLUME /composer/cache



