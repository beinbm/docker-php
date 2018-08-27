FROM debian:buster-slim
MAINTAINER Marco Beinbrech <marco@beinbrech.com>

### INSTALL APACHE & PHP
RUN applications="apache2 libapache2-mod-php7.2 php7.2 curl gnupg unzip" \
    && phpModules="php7.2-mysql php7.2-sqlite3 php7.2-intl php7.2-bcmath php7.2-curl php7.2-gmp php7.2-mbstring php7.2-xml php-xdebug" \
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
ADD ./docker/php.ini /etc/php/7.2/apache2/conf.d/zzz-settings.ini
ADD ./docker/php.ini /etc/php/7.2/cli/conf.d/zzz-settings.ini
RUN echo "\nxdebug.remote_enable=on\nxdebug.remote_autostart=on" >> /etc/php/7.2/mods-available/xdebug.ini

EXPOSE 80
WORKDIR /var/www/html
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
