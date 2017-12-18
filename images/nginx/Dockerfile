FROM phusion/baseimage
MAINTAINER Jacky <newbdez33@gmail.com>

# ensure UTF-8
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8

# Set the timezone.
RUN echo "Asia/Tokyo" > /etc/timezone
#RUN dpkg-reconfigure -f noninteractive tzdata
# change resolv.conf
#RUN echo 'nameserver 8.8.8.8' > /etc/resolv.conf

# setup
ENV HOME /root
RUN /etc/my_init.d/00_regen_ssh_host_keys.sh

CMD ["/sbin/my_init"]

# nginx-php installation
RUN DEBIAN_FRONTEND="noninteractive" apt-get update
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y upgrade
RUN DEBIAN_FRONTEND="noninteractive" apt-get update --fix-missing
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install php7.0
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install php7.0-fpm php7.0-common php7.0-cli php7.0-mysqlnd php7.0-mcrypt php7.0-curl php7.0-bcmath php7.0-mbstring php7.0-xml php7.0-zip php7.0-json php7.0-imap php-xdebug 
#php7.0-gearman

# install nginx (full)
RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y nginx-full

# install latest version of nodejs
RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y nodejs
RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y npm
RUN DEBIAN_FRONTEND="noninteractive" apt-get install -y git

# install php composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

# install MISC
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install cron
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install exiftool
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install curl
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install supervisor

# add build script
RUN mkdir -p /root/setup
ADD build/setup.sh /root/setup/setup.sh
RUN chmod +x /root/setup/setup.sh
RUN (cd /root/setup/; /root/setup/setup.sh)

# copy files from repo
ADD build/nginx.conf /etc/nginx/sites-available/default
ADD build/.bashrc /root/.bashrc

# disable services start
RUN update-rc.d -f apache2 remove
RUN update-rc.d -f nginx remove
RUN update-rc.d -f php7.0-fpm remove

# set WWW public folder
RUN mkdir -p /var/www/public
ADD build/index.php /var/www/public/index.php

RUN chown -R www-data:www-data /var/www
RUN chmod 755 /var/www

# set terminal environment
ENV TERM=xterm

# Add configuration files
ADD build/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD build/php.ini /etc/php/7.0/fpm/conf.d/40-custom.ini
ADD build/xextra.conf /etc/php/7.0/fpm/pool.d/xextra.conf
ADD build/custom.vhost /etc/nginx/sites-available/custom.conf
ADD build/crontab.root /var/spool/cron/crontabs/root

# Expose volumes
VOLUME ["/var/www"]

EXPOSE 80 9000

CMD ["/usr/bin/supervisord"]

# cleanup apt and lists
RUN apt-get clean
RUN apt-get autoclean


