# .devcontainer/Dockerfile
FROM php:8.2-apache

# --------------------------------------
# 1) Instalar dependencias del sistema
# --------------------------------------
RUN apt-get update && apt-get install -y \
    git zip unzip curl libzip-dev libpng-dev libonig-dev libxml2-dev \
    libicu-dev libsqlite3-dev libssl-dev \
  && rm -rf /var/lib/apt/lists/*

# --------------------------------------
# 2) Instalar extensiones PHP estándar
# --------------------------------------
# pdo_mysql, mbstring, xml, gd, curl, intl, zip, bcmath, soap, xmlrpc, opcache
RUN docker-php-ext-install \
    pdo pdo_mysql \
    mbstring \
    xml \
    gd \
    curl \
    intl \
    zip \
    bcmath \
    soap \
    xmlrpc \
    opcache

# --------------------------------------
# 3) Habilitar mod_rewrite de Apache
# --------------------------------------
RUN a2enmod rewrite

# --------------------------------------
# 4) Configurar DocumentRoot
# --------------------------------------
ENV APACHE_DOCUMENT_ROOT /var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' \
    /etc/apache2/sites-available/*.conf

# --------------------------------------
# 5) Copiar configuración de errores de PHP
# --------------------------------------
COPY php-errors.ini /usr/local/etc/php/conf.d/99-errors.ini

# --------------------------------------
# 6) Crear carpeta para logs y dar permisos
# --------------------------------------
RUN mkdir -p /var/log/php \
    && chown www-data:www-data /var/log/php

# --------------------------------------
# 7) Comando por defecto (opcional)
# --------------------------------------
CMD ["apache2-foreground"]
