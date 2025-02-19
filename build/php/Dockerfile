FROM php:8.3-apache

# Labels
LABEL creatorName="Alexey Chernyavskiy"
LABEL creatorEmail="alex@iit.kz"

# Set timezone
ENV TZ=${TZ:-UTC}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
        zip \
        libzip-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libwebp-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install -j$(nproc) mysqli pdo pdo_mysql opcache zip gd

# Copy php settings
COPY ./php.ini ${PHP_INI_DIR}/conf.d/99-php.ini

# Habilitar mod_rewrite para Laravel
RUN a2enmod rewrite && service apache2 restart

#Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

#Instalar Node JS
RUN curl - fsSL https://deb.nodesource.com/setup_22.x | bash -
RUN apt-get install -y nodejs