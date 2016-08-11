FROM drupal:8
MAINTAINER jean@oblady.fr

# Install packages.
RUN apt-get update && apt-get install -y \
  php5-cli && \
  apt-get clean
  
RUN { \
		echo 'upload_max_filesize = 64M'; \
		echo 'post_max_size = 64M'; \
	} > /usr/local/etc/php/conf.d/upload.ini

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

