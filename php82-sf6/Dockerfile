FROM php:8.2-fpm

# Arguments
ARG TZ=Europe/Paris

# Timezone
RUN ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime \
 && echo ${TZ} > /etc/timezone

# Dépendances système
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libpq-dev \
    libicu-dev \
    libzip-dev \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    && rm -rf /var/lib/apt/lists/*

# Extensions PHP
RUN docker-php-ext-configure intl \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install \
    pdo \
    pdo_pgsql \
    intl \
    zip \
    gd \
    opcache

# Redis (optionnel)
RUN pecl install redis \
 && docker-php-ext-enable redis

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# PHP configuration
COPY php.ini /usr/local/etc/php/php.ini

# Dossier de travail pour Symfony
WORKDIR /var/www/html/my_project_64

# Permissions (important pour Symfony)
RUN chown -R www-data:www-data /var/www/html/my_project_64

# Expose PHP-FPM port
EXPOSE 9000

# Lancer PHP-FPM
CMD ["php-fpm"]
