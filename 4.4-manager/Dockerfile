FROM php:7-apache

LABEL version="0.1.1" \
	description="Docker Image for a contao installation version 4.4.0" \
	maintainer="Holger Janßen-Kroll <docker@ho-jk.de>"


# install the PHP extensions we need
RUN set -ex; \
	\
	apt-get update; \
	apt-get install -y \
		libicu-dev \
		libjpeg-dev \
		libpng-dev \
		libgmp-dev \
		libmcrypt-dev \
		libxml2-dev \
		mysql-common \
		libbz2-dev \
		zlib1g-dev \
		vim \
	; \
	apt-get clean; \
	rm -rf /var/lib/apt/lists/*; \
	\
	pecl install mcrypt-1.0.1; \
	ln -s /usr/include/x86_64-linux-gnu/gmp.h /usr/local/include/; \
	export CFLAGS="-I/usr/src/php" ;\
	docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr; \
	docker-php-ext-install pdo pdo_mysql gd mysqli gmp bcmath zip xmlwriter xmlreader iconv bz2 mbstring soap intl 

RUN a2enmod rewrite expires


RUN set -ex; \
        # set the correct document root

        perl -pi -e "s#/var/www/html#/var/www/html/web#" /etc/apache2/sites-available/000-default.conf ; \
        perl -pi -e "s#/var/www/html#/var/www/html/web#" /etc/apache2/sites-available/default-ssl.conf ;\

	# set php timezone
	echo "date.timezone = 'Europe/Berlin'\n" >> /usr/local/etc/php/php.ini

RUN set -ex; \
	mkdir /var/www/html/web ;\
	curl https://download.contao.org/contao-manager.phar -o /var/www/html/web/contao-manager.phar.php ;\
	chown -R www-data:www-data  /var/www/html
	 
VOLUME /var/www/html


CMD ["apache2-foreground"]
