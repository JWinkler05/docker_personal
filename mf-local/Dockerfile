FROM ubuntu:12.04

MAINTAINER John Winkler <john.winkler@dialogtech.com>

# Update Base image
# Install needed dependencies for MongooseFramework

# Set Environment Variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

RUN apt-get -y update && \
  apt-get -y install sudo python-software-properties && \
  add-apt-repository -y ppa:ondrej/php5-oldstable && \
  apt-get -y update && \
  apt-get -y install supervisor openssh-server apache2 php5 curl php5-curl php5-mcrypt php5-mysql && \
  apt-get -y install php-pear php5-dev libapache-mod-ssl libssh2-1-dev libssh2-php git vim make && \
  mkdir -p $APACHE_LOCK_DIR /var/run/apache2 /var/run/sshd /var/log/supervisor && \
  apt-get remove --purge -y software-properties-common && \
  apt-get autoremove -y && \
  apt-get clean && \
  apt-get autoclean

# Install X-Debug
RUN sudo pecl install xdebug

# Copy the configuration file for the supervisor
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Make needed Directories
RUN mkdir /var/DialogTech && \
  mkdir /var/DialogTech/composer

# Install Composer
RUN curl -sS https://getcomposer.org/installer && \
  sudo php -- --install-dir=/var/DialogTech/composer --filename=composer

# Install Mail Mime for PHP
RUN pear install Mail_Mime

# Enable PHP Modules
RUN a2enmod php5 && \
  a2enmod rewrite && \
  a2enmod ssl

# Make the logging directory
RUN mkdir -p $APACHE_LOG_DIR

# Add the apache config from local to the sites enabled space.
ADD apache-config.conf /etc/apache2/sites-enabled/000-default.conf

# Install PHP Unit
RUN apt-get install wget && \
  wget https://phar.phpunit.de/phpunit-4.7.7.phar && \
  mv phpunit-4.7.7.phar phpunit.phar && \
  chmod +x phpunit.phar && \
  mv phpunit.phar /usr/local/bin/phpunit

RUN ln -sf /dev/stdout $APACHE_LOG_DIR/access.log
RUN ln -sf /dev/stderr $APACHE_LOG_DIR/error.log

WORKDIR /var/www/html/restapi.mongoosemetrics.com

EXPOSE 22 80 443

CMD ["/usr/bin/supervisord"]
