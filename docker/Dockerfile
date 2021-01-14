FROM php:7.4.13-fpm

# Set working directory
WORKDIR /var/www

RUN apt-get update && apt-get install -y \
	curl \
    libonig-dev \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libmagickwand-dev \
    iputils-ping

ADD xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql zip exif pcntl
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# PECL
RUN pecl install imagick
RUN pecl install xdebug-3.0.1
#RUN pecl install mongodb

# ENABLING EXTENSIONS
RUN docker-php-ext-enable imagick
RUN docker-php-ext-enable xdebug
#RUN docker-php-ext-enable mongodb

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory permissions
RUN chown -R www:www /var/www

COPY xdebug.log /tmp/logs/xdebug/xdebug.log
COPY --chown=www:www xdebug.log /tmp/logs/xdebug/xdebug.log
RUN chmod 777 /tmp/logs/xdebug/xdebug.log

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
