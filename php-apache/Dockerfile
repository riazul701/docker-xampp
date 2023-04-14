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
    && echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc \
    && mv composer.phar /usr/local/bin/composer

###########################################################################
# Node / NVM:
###########################################################################
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash \
    && export NVM_DIR="$HOME/.nvm" \
    && . $NVM_DIR/nvm.sh \
    && nvm install --lts \
    && nvm use --lts

RUN pecl channel-update pecl.php.net; \
  docker-php-ext-install mysqli pdo_mysql gd zip; \
  docker-php-ext-configure zip; \
  docker-php-ext-enable pdo_mysql gd zip;

RUN a2enmod rewrite; \
  a2enmod headers; \
  service apache2 restart;
