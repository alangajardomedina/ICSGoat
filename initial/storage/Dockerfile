FROM ubuntu:20.04
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update --fix-missing && \
    apt-get install -y --no-install-recommends \
    supervisor nano net-tools procps build-essential wget curl \
    php php-opcache php-gd php-curl php-mysql php-intl php-ldap \
    php-mbstring php-xml php-zip mariadb-client mariadb-server apache2 \
    autoconf dpkg-dev file g++ gcc libc-dev make pkg-config re2c unzip bash && \
    rm -rf /var/lib/apt/lists/*

COPY 000-default.conf /etc/apache2/sites-available/000-default.conf

WORKDIR /var/www/html

COPY owncloud /var/www/html/owncloud
COPY files /var/www/html/

RUN /etc/init.d/apache2 start \
        && a2enmod rewrite mime unique_id dir env headers mime rewrite setenvif \
        && /etc/init.d/apache2 restart

RUN /etc/init.d/mysql start && \
        sleep 4 && \
        echo "CREATE DATABASE IF NOT EXISTS owncloud;" | mysql && \
        echo "CREATE USER IF NOT EXISTS 'owncloud'@'localhost' IDENTIFIED BY 'password';" | mysql && \
        echo "GRANT ALL PRIVILEGES ON *.* TO 'owncloud'@'localhost' WITH GRANT OPTION;" | mysql && \
        echo "FLUSH PRIVILEGES;" | mysql

RUN rm -f /var/www/html/index.html && \
    mkdir -p /var/www/html/owncloud/data && \
    chmod -R 777 /var/www/html/owncloud/data && \
    chown -R www-data:www-data /var/www/html/owncloud/data && \
    mkdir -p /var/www/html/owncloud/apps-external && \
    chmod -R 777 /var/www/html/owncloud/apps-external

# FIX REAL
RUN apt-get update && apt-get install -y sudo

COPY start.sh /usr/local/bin/start.sh

RUN chmod +x /usr/local/bin/start.sh && \
    sed -i 's/\r$//' /usr/local/bin/start.sh

CMD ["/bin/bash", "/usr/local/bin/start.sh"]