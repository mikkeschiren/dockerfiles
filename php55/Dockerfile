
FROM debian:wheezy

ENV DEBIAN_FRONTEND noninteractive

# Install base deps:
RUN apt-get update && apt-get install -y --force-yes curl supervisor

# Install dotdeb repo:
RUN curl http://www.dotdeb.org/dotdeb.gpg | apt-key add -
ADD dotdeb-php55.list /etc/apt/sources.list.d/dotdeb-php55.list

# Install php deps:
RUN apt-get update && apt-get install -y --force-yes apache2 php5 php5-curl php5-mysqlnd php5-gd
RUN a2dissite default && a2enmod rewrite && a2enmod php5

# Configure:
RUN sed -i "s/; date.timezone =/date.timezone = America\/New_York/" /etc/php5/cli/php.ini /etc/php5/apache2/php.ini
ADD apache-vhost.conf /etc/apache2/sites-enabled/default
ADD supervisor.conf /etc/supervisor/conf.d/supervisor.conf
RUN echo "opcache.memory_consumption = 128" >> /etc/php5/mods-available/opcache.ini

# Cleanup:
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

VOLUME ["/srv"]
WORKDIR /srv
EXPOSE 80

CMD ["/usr/bin/supervisord"]
