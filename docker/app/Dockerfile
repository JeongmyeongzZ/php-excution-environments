FROM php:7.4-cli

RUN set -xe; \
    apt-get update && \
    apt-get install -y curl  zip  zlib1g-dev libzip-dev libicu-dev && \
    docker-php-ext-install zip intl

ARG APP_USER_ID=1000
ARG APP_GROUP_ID=1000

RUN set -xe; \
    groupadd -f app && \
    groupmod -g ${APP_GROUP_ID} app && \
    useradd app -g app && \
    mkdir -p /home/app && chmod 755 /home/app && chown app:app /home/app && \
    usermod -u ${APP_USER_ID} -m -d /home/app app -s $(which bash) && \
    chown -R app:app /var/www/html

COPY config/* $PHP_INI_DIR/conf.d/

ARG TIME_ZONE='Asia/Seoul'

RUN ln -snf /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && echo ${TIME_ZONE} > /etc/timezone

ARG COMPOSER_VERSION=2.0.2
ARG COMPOSER_REPO_PACKAGIST='https://packagist.jp'

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer --version=${COMPOSER_VERSION} && \
    composer config -g repos.packagist composer ${COMPOSER_REPO_PACKAGIST}

WORKDIR /var/www/html
