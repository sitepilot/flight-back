FROM ubuntu:20.04
LABEL maintainer "Sitepilot <support@sitepilot.io>"

ENV RUNTIME_USER_ID=10000
ENV RUNTIME_GROUP_ID=10000

ENV PHP_VERSION=74
ENV PATH="/opt/runtime/bin:${PATH}"
ENV COMPOSER_HOME=/opt/runtime/app/.composer

# ----- Build Files ----- #

COPY build /

# ----- Packages ----- #

RUN install-packages gpg-agent software-properties-common supervisor curl wget nano libnss3-tools

# ----- Openlitespeed & PHP ----- #

RUN wget -O - http://rpms.litespeedtech.com/debian/enable_lst_debian_repo.sh | bash

RUN install-packages \
    lsphp$PHP_VERSION \
    lsphp$PHP_VERSION-mysql \
    lsphp$PHP_VERSION-imap \
    lsphp$PHP_VERSION-curl \
    lsphp$PHP_VERSION-common \
    lsphp$PHP_VERSION-json \
    lsphp$PHP_VERSION-redis \
    lsphp$PHP_VERSION-opcache \
    lsphp$PHP_VERSION-igbinary \
    lsphp$PHP_VERSION-imagick \
    lsphp$PHP_VERSION-intl \
    openlitespeed

RUN ln -sf /usr/local/lsws/lsphp$PHP_VERSION/bin/php /usr/local/bin/php \
    && ln -sf /opt/runtime/config/litespeed.conf /usr/local/lsws/conf/httpd_config.conf

# ----- Caddy ----- #

RUN curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | apt-key add - \
    && curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee -a /etc/apt/sources.list.d/caddy-stable.list \
    && install-packages caddy

# ----- Composer ----- #

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && mv composer.phar /usr/local/bin/composer \
    && php -r "unlink('composer-setup.php');" \
    && composer --version

# ----- NodeJS ----- #

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && install-packages nodejs \
    && npm -v \
    && node -v \
    && npm install -g yarn

# ----- WPCLI ----- #

RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp \
    && wp --allow-root --version

# ------ User ----- #

RUN usermod -u ${RUNTIME_USER_ID} -d /opt/runtime/app www-data \
    && groupmod -g ${RUNTIME_GROUP_ID} www-data \
    && chsh -s /bin/bash www-data

# ----- Files ----- #

COPY filesystem /

RUN mkdir -p /var/run \
    && mkdir -p /opt/runtime/etc \
    && chown -R www-data:www-data /run \
    && chown -R www-data:www-data /opt/runtime \
    && chown -R www-data:www-data /usr/local/lsws \
    && ln -sf /dev/stderr /usr/local/lsws/logs/error.log \
    && ln -sf /dev/stderr /usr/local/lsws/logs/stderr.log

# ----- Config ----- #

EXPOSE 80
EXPOSE 443
EXPOSE 8080

WORKDIR /opt/runtime/app

ENTRYPOINT ["/opt/runtime/bin/entrypoint"]

CMD ["supervisord", "-c", "/opt/runtime/config/supervisor.conf"]
