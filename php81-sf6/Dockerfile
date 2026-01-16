# Base PHP-FPM 8.1
FROM php:8.1-fpm

# Définir le répertoire de travail
WORKDIR /var/www/html

# Installer les dépendances système et extensions PHP nécessaires
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    curl \
    vim \
    less \
    procps \
    libicu-dev \
    libpq-dev \
    libzip-dev \
    libonig-dev \
    zlib1g-dev \
    libxml2-dev \
    && docker-php-ext-install intl pdo pdo_mysql pdo_pgsql zip opcache bcmath xml \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# Installer Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Copier le projet (optionnel, si volume monté, peut être ignoré)
COPY . /var/www/html

# Créer var et vendor si inexistants + permissions sécurisées
RUN mkdir -p /var/www/html/var /var/www/html/vendor \
    && chown -R www-data:www-data /var/www/html/var /var/www/html/vendor \
    && chmod -R 755 /var/www/html

# Exposer le port PHP-FPM
EXPOSE 9000

# Lancer PHP-FPM
CMD ["php-fpm"]