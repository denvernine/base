# syntax=docker/dockerfile:1-labs
FROM php:8.2-apache-bullseye

RUN a2enmod rewrite

COPY --from=composer/composer:lts /usr/bin/composer /usr/bin/composer

ARG UNAME
ARG UID
ARG GID=${UID}

RUN <<end_of_run
  groupadd -g ${GID} -- ${UNAME}
  useradd -u ${UID} -g ${GID} -s ${LOGIN_SHELL:-/bin/bash} --no-create-home --home-dir /var/tmp -- ${UNAME}
end_of_run

RUN <<end_of_run
  apt-get update
  apt-get -y --no-install-recommends install \
      ca-certificates \
      curl \
      default-mysql-client-core \
      dnsutils \
      git \
      gnupg \
      gosu \
      libcap2-bin \
      libicu-dev \
      libonig-dev \
      libpng-dev \
      librsvg2-bin \
      libzip-dev \
      python2 \
      sqlite3 \
      supervisor \
      unzip \
      zip
  apt-get clean
  rm -rf /var/lib/apt/lists/*
end_of_run
RUN <<end_of_run
  docker-php-ext-install \
      bcmath \
      intl \
      opcache \
      pdo_mysql \
      sockets \
      zip
end_of_run

COPY apache2/mpm_prefork.conf /etc/apache2/mods-enabled/mpm_prefork.conf
COPY apache2/ports.conf /etc/apache2/ports.conf
COPY apache2/vhost.conf /etc/apache2/sites-enabled/000-default.conf

COPY php/php.ini /usr/local/etc/php/php.ini
COPY php/conf.d/docker-php-ext-opcache.ini /usr/local/etc/php/conf.d/docker-php-ext-opcache.ini

CMD ["apache2", "-D", "FOREGROUND"]
