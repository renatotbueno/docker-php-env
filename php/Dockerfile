FROM php:7.2.2-fpm

LABEL maintainer="Renato Teixeira Bueno <renatotbueno@gmail.com>"

RUN apt-get update && apt-get upgrade -y \
    g++ \
    libc-client-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libkrb5-dev \
    libpq-dev \
    libmagickwand-dev \
    libmcrypt-dev \
    libpng-dev \
    libmemcached-dev \
    libssl-dev \
    libssl-doc \
    libsasl2-dev \
    zlib1g-dev \
    && docker-php-ext-install \
    bz2 \
    iconv \
    mbstring \
    mysqli \
    pgsql \
    pdo_mysql \
    pdo_pgsql \
    soap \
    zip \
    && docker-php-ext-configure gd \
    --with-freetype-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && yes '' | pecl install imagick && docker-php-ext-enable imagick \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && pecl install memcached && docker-php-ext-enable memcached \
    && pecl install mongodb && docker-php-ext-enable mongodb \
    && pecl install redis && docker-php-ext-enable redis \
    && pecl install xdebug && docker-php-ext-enable xdebug \
    && apt-get autoremove -y --purge \
    && apt-get clean \
    && rm -Rf /tmp/*

ADD custom_php.ini /usr/local/etc/php/conf.d/
RUN apt-get update && apt-get install -my wget gnupg
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN cp -rv composer.phar /usr/bin/composer
RUN chmod +x /usr/bin/composer
RUN curl -sL https://deb.nodesource.com/setup_8.x |  bash -
RUN apt install -y nodejs
RUN npm install -g bower
RUN apt install git -y
RUN npm install -g gulp

# Authorize SSH Host
RUN mkdir -p /personal_key/.ssh && \
    chmod 0700 /personal_key/.ssh && \
    ssh-keyscan bitbucket.org > /personal_key/.ssh/known_hosts

COPY .ssh/id_rsa /personal_key/.ssh/id_rsa
COPY .ssh/id_rsa.pub /personal_key/.ssh/id_rsa.pub

# set permissions
RUN chmod 600 /personal_key/.ssh/id_rsa && \
    chmod 600 /personal_key/.ssh/id_rsa.pub

RUN  echo "IdentityFile /personal_key/.ssh/id_rsa" >> /etc/ssh/ssh_config

CMD ["php-fpm", "-g", "daemon off;"]
