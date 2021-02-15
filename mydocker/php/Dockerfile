FROM php:7.4.12-fpm
WORKDIR "/app"

# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils

# Update packages & Install Git, ffmpeg and supervisor
RUN apt-get update && apt-get install -yq \
  git \
  ffmpeg \
  supervisor

RUN apt-get install -yq \
  apt-utils \
  libzip-dev \
  libpng-dev \
  libjpeg-dev \
  libfreetype6-dev \
  libpq-dev \
  libtool \
  jpegoptim \
  optipng \
  pngquant \
  webp

# Install gd extension (PHP)
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j "$(nproc)" gd

# Install extensions
RUN docker-php-ext-install \
  opcache \
  zip

# Install extensions by PECL
RUN pecl install \
  redis

# Manually enable extensions
RUN docker-php-ext-enable \
  redis

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Use the default production configuration
RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

# Setup OPCache
ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS="0"
ADD opcache.ini "$PHP_INI_DIR/conf.d/opcache.ini"

# Setup Supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisor.conf
CMD ["/usr/bin/supervisord", "-n"]