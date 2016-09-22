FROM ubuntu:12.04

MAINTAINER John Winkler <john.winkler@dialogtech.com>

# Set Environment Variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/logs/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

RUN apt-get -y update && \
  apt-get -y install sudo python-software-properties && \
  add-apt-repository -y ppa:ondrej/php5-oldstable && \
  apt-get -y update && \
  apt-get -y install apache2 php5 curl php5-curl php5-mcrypt php5-mysql && \
  apt-get -y install php-pear php5-dev

# Add the apache config from local to the sites enabled space.
ADD apache2/apache-config.conf /etc/apache2/sites-enabled/000-default.conf

RUN mkdir -p $APACHE_LOG_DIR && \
  touch $APACHE_LOG_DIR/access.log && \
  touch $APACHE_LOG_DIR/error.log && \
  ln -sf /dev/stdout $APACHE_LOG_DIR/access.log && \
  ln -sf /dev/stderr $APACHE_LOG_DIR/error.log

# Enable PHP Modules
RUN a2enmod php5

EXPOSE 22 80 443

CMD /usr/sbin/apache2ctl -D FOREGROUND
