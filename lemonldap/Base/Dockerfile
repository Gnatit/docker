# Dockerfile for LemonLDAP::NG
# Use debian repo of LemonLDAP::NG project
# Adapted from work by oudot
# https://github.com/coudot/lemonldap-ng-docker

# Start from Debian Jessie
FROM debian:jessie
MAINTAINER Tanguy Le Berre

# Update system
RUN apt-get -y update && apt-get -y dist-upgrade

# Install LemonLDAP::NG repo
RUN apt-get -y install wget
RUN wget -O - http://lemonldap-ng.org/_media/rpm-gpg-key-ow2 | apt-key add -
COPY lemonldap-ng.list /etc/apt/sources.list.d/

# Install LemonLDAP::NG packages
RUN apt-get -y update && apt-get -y install apache2 libapache2-mod-perl2 libapache2-mod-fcgid lemonldap-ng lemonldap-ng-fr-doc liblasso-perl vim ack-grep libdbd-pg-perl

# Configure mod
RUN echo '<IfModule mod_rewrite.c>\n\
        RewriteEngine On\n\
        RewriteRule ^/saml/metadata /metadata.pl\n\
        RewriteRule ^/saml/.* /index.pl\n\
</IfModule>'\
>> /etc/apache2/mods-available/rewrite.conf

# Enable mods
RUN a2dismod rewrite
RUN a2enmod fcgid perl alias rewrite proxy_http rewrite

# Set to debug
RUN sed -i "s/LogLevel warn/LogLevel debug/g" /etc/apache2/apache2.conf

# File to safely remove pid
RUN echo '#!/bin/bash\n\
rm  /var/run/apache2/apache2.pid\n\
/usr/sbin/apache2ctl -D FOREGROUND'\
>> /usr/bin/safe_apache

RUN chmod +x /usr/bin/safe_apache

EXPOSE 80 443
ENTRYPOINT ["/usr/bin/safe_apache"]
