FROM drupal:8
MAINTAINER jean@oblady.fr

# Install packages.
RUN apt-get update && apt-get install -y \
  php5-cli \
  nano \
  curl && \
  apt-get clean
  
RUN { \
		echo 'upload_max_filesize = 64M'; \
		echo 'post_max_size = 64M'; \
		echo 'display_errors = On'; \
	} > /usr/local/etc/php/conf.d/obladycustom.ini

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# Install drupal console
RUN curl https://drupalconsole.com/installer -L -o drupal.phar && \
    mv drupal.phar /usr/local/bin/drupal && \
    composer require drupal/console:~1.0 --prefer-dist --optimize-autoloader
   
# Install drush
RUN php -r "readfile('https://s3.amazonaws.com/files.drush.org/drush.phar');" > drush && \
    chmod +x drush && \
    mv drush /usr/local/bin

# Install Solar API
RUN composer config repositories.drupal composer https://packagist.drupal-composer.org && \
    composer require "drupal/search_api_solr 8.1.0-beta1"

# Allow social timeline scripts to be executed
RUN echo '' >> .htaccess && \
    echo '<IfModule mod_rewrite.c>' >> .htaccess && \
    echo '    RewriteCond %{REQUEST_URI} !/modules/social_timeline/library/youtube_auth/youtube.php$' >> .htaccess && \
    echo '    RewriteCond %{REQUEST_URI} !/modules/social_timeline/library/twitter_oauth/user_timeline.php$' >> .htaccess && \
    echo '</IfModule>' >> .htaccess
