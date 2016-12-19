# Kerberos apache2

# Start from Debian Jessie
FROM debian:jessie
MAINTAINER Tanguy Le Berre

# Update system
RUN apt-get -y update && apt-get -y dist-upgrade

# Install Apache 2 and packages
RUN apt-get -y update && DEBIAN_FRONTEND=noninteractive apt-get -y install apache2 libapache2-mod-perl2 vim ack-grep libapache2-mod-auth-kerb

# Enable mods
RUN a2enmod ssl auth_kerb

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
