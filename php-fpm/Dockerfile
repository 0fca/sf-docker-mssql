FROM php:7.4.10-fpm-buster

LABEL maintainer="Łukasz Bownik <lukasbownik99@gmail.com>"
ENV ACCEPT_EULA=Y

RUN apt-get update && \
    apt-get -y install gnupg2
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - && \
    curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list && \
    apt-get update && \
    apt-get -y install \
    coreutils \
    make \
    g++ \
    git \
    libz-dev \
    libzip-dev \
    openssl \
    libxml2-dev \
    libwebp-dev \
    libjpeg62-turbo-dev \
    libpng-dev libxpm-dev \
    libfreetype6-dev \
    libc-client-dev \
    libcurl4-openssl-dev \
    unzip \
    libpng-dev \
    zlib1g-dev \
    libicu-dev \
    unixodbc-dev \
    curl \
    msodbcsql17 \ 
    unixodbc-dev \
    libgssapi-krb5-2 && \
    apt-get clean

RUN pecl install sqlsrv && pecl install pdo_sqlsrv

RUN docker-php-ext-install \
    opcache \
    curl \ 
    zip \
    pdo \
    exif \
    pcntl \
    intl \
    dom \
    simplexml \
    xml \
    gd

RUN echo "; priority=20\nextension=sqlsrv.so\n" >> /usr/local/etc/php/php.ini
RUN echo "; priority=30\nextension=pdo_sqlsrv.so\n" >> /usr/local/etc/php/php.ini
RUN echo "memory_limit = 512M" >> /usr/local/etc/php/php.ini

RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
        && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
        && php composer-setup.php && rm composer-setup.php* \
        && chmod +x composer.phar && mv composer.phar /usr/bin/composer

COPY symfony.ini /etc/php7.4/conf.d/
COPY symfony.ini /etc/php7.4/cli/conf.d/
COPY xdebug.ini  /etc/php7.4/conf.d/

COPY symfony.pool.conf /etc/php7.4/php-fpm.d/

CMD ["php-fpm", "-F"]

WORKDIR /var/www/symfony
EXPOSE 9001
