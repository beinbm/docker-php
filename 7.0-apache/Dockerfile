FROM debian:stretch-slim
MAINTAINER Marco Beinbrech <marco@beinbrech.com>

### INSTALL APACHE & PHP
RUN applications="apache2 libapache2-mod-php7.0 php7.0 curl gnupg unzip" \
    && phpModules="php7.0-mysql php7.0-sqlite3 php7.0-mcrypt php7.0-intl php7.0-bcmath php7.0-curl php7.0-gmp php7.0-mbstring php7.0-xml php-xdebug php7.0-sqlite3" \
    && apt-get update \
    && apt-get install -y $applications $phpModules \
    && rm -rf /var/lib/apt/lists/*

RUN ln -sf /dev/stdout /var/log/apache2/access.log && ln -sf /dev/stderr /var/log/apache2/error.log
RUN a2enmod rewrite
RUN usermod -u 1000 www-data

# https://getcomposer.org/doc/03-cli.md#composer-allow-superuser
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

### SETUP CONTAINER
ADD ./docker/vhost.conf /etc/apache2/sites-available/000-default.conf
ADD ./docker/php.ini /etc/php/7.0/apache2/conf.d/zzz-settings.ini
ADD ./docker/php.ini /etc/php/7.0/cli/conf.d/zzz-settings.ini
RUN echo "\nxdebug.remote_enable=on\nxdebug.remote_autostart=on" >> /etc/php/7.0/mods-available/xdebug.ini

EXPOSE 80
WORKDIR /var/www/html
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]