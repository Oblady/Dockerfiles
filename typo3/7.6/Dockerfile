FROM ubuntu:xenial

MAINTAINER Jean Traullé <jean@oblady.fr>

ENV DEBIAN_FRONTEND noninteractive

RUN locale-gen fr_FR.UTF-8
ENV LANG=fr_FR.UTF-8 LANGUAGE=fr_FR:fr LC_ALL=fr_FR.UTF-8

RUN apt-get update -qqy && \
    apt-get install -qqy \
    nginx \
    graphicsmagick \
    graphicsmagick-imagemagick-compat \
    composer \
    php-fpm \
    php-common \
    php-mbstring \
    php-curl \
    php-mcrypt \
    php-soap \
    php-xml \
    php-zip \
    php-mysql \
    php-intl \
    php-json \
    php-gd \
    php-imagick \
    php-xdebug \
    curl \
    sendmail && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

ADD ./nginx.conf /etc/nginx/sites-available/default.conf

RUN sed -i -e "s/worker_processes  1/worker_processes 5/" /etc/nginx/nginx.conf && \
    sed -i -e "s/keepalive_timeout 2/keepalive_timeout 2;\n\tclient_max_body_size 100m/" /etc/nginx/nginx.conf && \
    echo "daemon off;" >> /etc/nginx/nginx.conf && \
    sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" /etc/php/7.0/fpm//php.ini && \
    sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/upload_max_filesize\s*=\s*2M/upload_max_filesize = 100M/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/post_max_size\s*=\s*8M/post_max_size = 100M/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/max_execution_time\s*=\s*30/max_execution_time = 240/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/; max_input_vars\s*=\s*1000/max_input_vars = 1500/g" /etc/php/7.0/fpm/php.ini && \
    sed -i -e "s/;opcache.enable\s*=\s*0/opcache.enable=1/g" /etc/php/7.0/fpm/php.ini && \
    echo "xdebug.max_nesting_level=400" >> /etc/php/7.0/fpm/conf.d/20-xdebug.ini && \
    echo "php_flag[display_errors] = on" >> /etc/php/7.0/fpm/pool.d/www.conf && \
    echo "php_flag[display_startup_errors] = on" >> /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i -e "s/;daemonize\s*=\s*yes/daemonize = no/g" /etc/php/7.0/fpm/php.ini;

RUN usermod -u 1000 www-data && \
    usermod -a -G users www-data && \
    rm -Rf /etc/nginx/conf.d/* && \
    rm -Rf /etc/nginx/sites-available/default && \
    ln -s /etc/nginx/sites-available/default.conf /etc/nginx/sites-enabled/default.conf && \
    ln -sf /dev/stdout /var/log/nginx/access.log && \
    ln -sf /dev/stderr /var/log/nginx/error.log && \
    ln -s /usr/bin/nodejs /usr/bin/node && \
    mkdir -p /var/www/html && \
    mkdir /var/www/html/typo3temp /var/www/html/typo3conf /var/www/html/fileadmin /var/www/html/uploads /run/php && \
    chown -R www-data:users /var/www/html

WORKDIR /var/www/html
VOLUME ["/var/www/html/typo3conf", "/var/www/html/fileadmin", "/var/www/html/uploads"]

# Set this to the latest TYPO3 CMS version:
ENV TYPO3_VERSION 7.6.9

RUN curl -L  http://prdownloads.sourceforge.net/typo3/typo3_src-${TYPO3_VERSION}.tar.gz | tar zxf - && \
    ln -s typo3_src-${TYPO3_VERSION} typo3_src && \
    ln -s typo3_src/index.php index.php && \
    ln -s typo3_src/typo3 typo3

EXPOSE 80

CMD /usr/sbin/php-fpm7.0 -D && /usr/sbin/nginx
