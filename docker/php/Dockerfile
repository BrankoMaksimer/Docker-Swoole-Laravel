FROM php:7.4-fpm
WORKDIR "/code"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

# Update packages & Install Git and Supervisor
RUN apt-get update -y && \
    apt-get install -yq git supervisor

# Install GD extension
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

RUN install-php-extensions gd

# Install Swoole extension
RUN pecl install swoole
RUN docker-php-ext-enable swoole

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Added supervisor config
COPY supervisord.conf /etc/supervisor/conf.d/supervisor.conf
CMD ["/usr/bin/supervisord", "-n"]

#Install mysql
RUN docker-php-ext-install pdo pdo_mysql mysqli

#Install Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=. --filename=composer

