FROM php:7.4-fpm

# apt-get update then install
RUN apt-get update && apt-get install -y \
    # ext-libsodium
    libsodium-dev \
    # Git
    git \
    # Supervisor
    supervisor \
    # ps
    procps \
    # Zip Unzip
    zip unzip 

# Redis XDebug
RUN pecl install redis xdebug

# Install extenstions
RUN docker-php-ext-install mysqli opcache pdo pdo_mysql sodium

# Enable extensions
RUN docker-php-ext-enable opcache redis xdebug

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php && \
mv composer.phar /usr/local/bin/composer && \
# Composer cache
mkdir -p /var/www/.composer && \
chmod -R 777 /var/www/.composer

# Settings overrides
COPY php.overrides.ini /usr/local/etc/php/conf.d/php.overrides.ini
COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Set timezone
ENV TZ=Asia/Kuala_Lumpur
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Add custom supervisor config
ADD supervisor /etc/supervisor/conf.d
RUN touch /var/run/supervisor.sock && chmod 777 /var/run/supervisor.sock

# Our start script
ADD start.sh /
RUN chmod 755 /start.sh

# Permissions settings for docker
# RUN usermod -u 1000 www-data
# USER www-data

# Run the start script
ENTRYPOINT ["sh", "/start.sh"]