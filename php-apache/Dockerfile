ARG PHP_VERSION
ARG LARADOCK_PHP_VERSION
FROM php:${LARADOCK_PHP_VERSION}-apache

RUN set -eux \
    #--------------------------------------------------------------------------
    # Software's Installation
    #--------------------------------------------------------------------------
    #
    && apt-get update \
    #&& apt-get upgrade -y \
    && apt-get install -y --allow-downgrades --allow-remove-essential \
        --allow-change-held-packages \
        rsync \
        sqlite3 \
        git \
        curl \
        vim \
        nano \
        tree \
        libgd-dev libzip-dev zip unzip libpng-dev \
    && apt-get clean \
    #####################################
    # Composer:
    #####################################
    # Install composer and add its bin to the PATH.
    && curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer

###########################################################################
# Node / NVM:
###########################################################################
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && export NVM_DIR="$HOME/.nvm" \
    && . $NVM_DIR/nvm.sh \
    && nvm install --lts \
    && nvm use --lts

###########################################################################
# [PHP Extensions](https://github.com/mlocati/docker-php-extension-installer)
###########################################################################

RUN pecl channel-update pecl.php.net; \
  docker-php-ext-install mysqli pdo_mysql gd zip; \
  docker-php-ext-configure zip; \
  docker-php-ext-enable pdo_mysql gd zip;

RUN pecl install xdebug; \
  cd /usr/local/etc/php && \
  cp php.ini-development php.ini && \
  echo '[xdebug]' >> php.ini && \
  echo 'zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20220829/xdebug.so' >> php.ini && \
  echo 'xdebug.mode=debug' >> php.ini && \
  echo 'xdebug.start_with_request=yes' >> php.ini && \
  echo 'xdebug.client_host="host.docker.internal"' >> php.ini

###########################################################################
# Apache2 Configuration:
###########################################################################

RUN a2enmod rewrite; \
  a2enmod headers; \
  service apache2 restart;
