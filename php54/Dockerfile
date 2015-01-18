
FROM debian:wheezy

ENV DEBIAN_FRONTEND noninteractive

# Install base deps
RUN apt-get update && apt-get install -y --force-yes curl supervisor

# Install php deps
RUN apt-get update && apt-get install -y --force-yes apache2 php5 php5-curl php5-mysqlnd php5-gd php-apc
RUN a2dissite default && a2enmod rewrite && a2enmod php5

# Configure:
RUN sed -i "s/; date.timezone =/date.timezone = America\/New_York/" /etc/php5/cli/php.ini /etc/php5/apache2/php.ini
ADD apache-vhost.conf /etc/apache2/sites-enabled/default
ADD supervisor.conf /etc/supervisor/conf.d/supervisor.conf
RUN echo "apc.shm_size = 128M" >> /etc/php5/mods-available/apc.ini

# Cleanup:
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME ["/srv"]
WORKDIR /srv
EXPOSE 80

CMD ["/usr/bin/supervisord"]
