FROM drupal:8
MAINTAINER jean@oblady.fr

# Install packages.
RUN apt-get update && apt-get install -y \
  php5-cli && \
  apt-get clean

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

