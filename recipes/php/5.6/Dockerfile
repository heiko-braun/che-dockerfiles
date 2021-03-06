FROM eclipse/stack-base:debian
ENV DEBIAN_FRONTEND noninteractive
ENV CHE_MYSQL_PASSWORD=che
ENV CHE_MYSQL_DB=che_db
ENV CHE_MYSQL_USER=che
RUN sudo apt-get update && \
    sudo apt-get -y --no-install-recommends install mysql-server \
    apache2 \
    php5 \
    php5-mhash \
    php5-mcrypt \
    php5-curl \
    php5-cli \
    php5-mysql \
    php5-gd \
    libapache2-mod-php5 \
    php5-cli \
    php5-json \
    php5-cgi \
    php5-sqlite && \
    sudo sed -i 's/\/var\/www\/html/\/projects/g'  /etc/apache2/sites-available/000-default.conf && \
    sudo sed -i 's/None/All/g' /etc/apache2/apache2.conf && \
    sudo sed -i 's/\/var\/www/\/projects/g'  /etc/apache2/apache2.conf && \
    echo "ServerName localhost" | sudo tee -a /etc/apache2/apache2.conf && \
    sudo a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer && \
    sudo composer global require bamarni/symfony-console-autocomplete && \
    ~/.composer/vendor/bamarni/symfony-console-autocomplete/symfony-autocomplete --shell bash composer | sudo tee /etc/bash_completion.d/composer && \
    sudo wget -qO /usr/local/bin/phpunit https://phar.phpunit.de/phpunit.phar && sudo chmod +x /usr/local/bin/phpunit && \
    echo -e "MySQL password: $CHE_MYSQL_PASSWORD" >> /home/user/.mysqlrc && \
    echo -e "MySQL user    : $CHE_MYSQL_USER" >> /home/user/.mysqlrc && \
    echo -e "MySQL Database: $CHE_MYSQL_DB" >> /home/user/.mysqlrc && \
    sudo -E bash -c "apt-get -y --no-install-recommends install mysql-server" && \
    sudo apt-get clean && \
    sudo apt-get -y autoremove && \
    sudo apt-get -y clean && \
    sudo rm -rf /var/lib/apt/lists/* && \
    sudo sed -i.bak 's/127.0.0.1/0.0.0.0/g' /etc/mysql/my.cnf && \
    sudo service mysql start && sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost'; FLUSH PRIVILEGES;" && \
    sudo service mysql restart && \
    sudo service mysql restart && sudo mysql -uroot -e "CREATE USER '$CHE_MYSQL_USER'@'%' IDENTIFIED BY '"$CHE_MYSQL_PASSWORD"'" && \
    sudo mysql -uroot -e "GRANT ALL PRIVILEGES ON *.* TO '$CHE_MYSQL_USER'@'%' IDENTIFIED BY '"$CHE_MYSQL_PASSWORD"'; FLUSH PRIVILEGES;" && \
    sudo mysql -uroot -e "CREATE DATABASE $CHE_MYSQL_DB;"
EXPOSE 80