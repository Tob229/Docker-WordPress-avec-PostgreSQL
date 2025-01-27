FROM wordpress:php8.1-apache

# Installer les dépendances
RUN apt-get update \
    && apt-get install -y git curl libpq-dev \
    && docker-php-ext-configure pgsql --with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql \
    && rm -rf /var/lib/apt/lists/*

# Télécharger et extraire wp-config-sample.php
WORKDIR /tmp
RUN curl -O https://wordpress.org/latest.tar.gz \
    && tar -xzf latest.tar.gz --strip-components=1 wordpress/wp-config-sample.php \
    && rm latest.tar.gz \
    && mv wp-config-sample.php /var/www/html/

# Installer Pg4wp
WORKDIR /var/www/html/wp-content
RUN git clone https://github.com/kevinoid/postgresql-for-wordpress.git \
    && mv postgresql-for-wordpress/pg4wp ./ \
    && rm -rf postgresql-for-wordpress \
    && cp pg4wp/db.php ./

# Configurer wp-config.php
WORKDIR /var/www/html
RUN chown -R root:root /var/www/html \
    && mv wp-config-sample.php wp-config.php \
    && sed -i "s/database_name_here/' . getenv(\"WORDPRESS_DB_NAME\") . '/" wp-config.php \
    && sed -i "s/username_here/' . getenv(\"WORDPRESS_DB_USER\") . '/" wp-config.php \
    && sed -i "s/password_here/' . getenv(\"WORDPRESS_DB_PASSWORD\") . '/" wp-config.php \
    && sed -i "s/localhost/' . getenv(\"WORDPRESS_DB_HOST\") . '/" wp-config.php \
    && sed -i "/AUTH_KEY/d;/SECURE_AUTH_KEY/d;/LOGGED_IN_KEY/d;/NONCE_KEY/d;/AUTH_SALT/d;/SECURE_AUTH_SALT/d;/LOGGED_IN_SALT/d;/NONCE_SALT/d" wp-config.php \
    && curl -s https://api.wordpress.org/secret-key/1.1/salt/ | sed 's/\r//g' > /tmp/salt.txt \
    && sed -i '/\*\*#@+/r /tmp/salt.txt' wp-config.php \
    && rm /tmp/salt.txt \
    && chown -R www-data:www-data /var/www/html
