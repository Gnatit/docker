# Dockerfile for LemonLDAP::NG
# Use debian repo of LemonLDAP::NG project
# Adapted from work by oudot
# https://github.com/coudot/lemonldap-ng-docker

# Start from Debian Jessie
FROM lemon/base
MAINTAINER Tanguy Le Berre


# Change SSO DOMAIN here
ARG SSODOMAIN=lemon.idp.fr

# Change SSO Domain
RUN sed -i "s/example\.com/${SSODOMAIN}/g" /etc/lemonldap-ng/* /var/lib/lemonldap-ng/conf/lmConf-1.js /var/lib/lemonldap-ng/test/index.pl

# Enable sites
RUN a2ensite handler-apache2.conf
RUN a2ensite portal-apache2.conf
RUN a2ensite manager-apache2.conf
RUN a2ensite test-apache2.conf

# Remove cached configuration
RUN rm -rf /tmp/lemonldap-ng-config

EXPOSE 80 443
VOLUME ["/var/log/apache2", "/etc/apache2", "/etc/lemonldap-ng", "/var/lib/lemonldap-ng/conf", "/var/lib/lemonldap-ng/sessions", "/var/lib/lemonldap-ng/psessions"]
ENTRYPOINT ["/usr/bin/safe_apache"]
