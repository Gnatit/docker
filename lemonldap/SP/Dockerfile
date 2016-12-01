# Dockerfile for LemonLDAP::NG
# Use debian repo of LemonLDAP::NG project
# Adapted from work by oudot
# https://github.com/coudot/lemonldap-ng-docker

# Start from Debian Jessie
FROM lemon/base
MAINTAINER Tanguy Le Berre

# Change SSO DOMAIN here
ARG SSODOMAIN=lemon.sp.fr
ARG SERVICE_NAME=tomcat
ARG SERVICE_URL=${SERVICE_NAME}.fr:8080

# Change SSO Domain
RUN sed -i "s/example\.com/${SSODOMAIN}/g" /etc/lemonldap-ng/* /var/lib/lemonldap-ng/conf/lmConf-1.js /var/lib/lemonldap-ng/test/index.pl

# Add service site
RUN echo "<VirtualHost *:80>\n\
        ServerName ${SERVICE_NAME}.${SSODOMAIN}\n\
 \n\
        PerlHeaderParserHandler Lemonldap::NG::Handler\n\
 \n\
        # Reverse-Proxy\n\
        ProxyPass / http://${SERVICE_URL}/\n\
        # Change "Location" header in redirections\n\
        ProxyPassReverse / http://${SERVICE_URL}/\n\
        # Change domain cookies\n\
        ProxyPassReverseCookieDomain ${SERVICE_NAME}.fr ${SERVICE_NAME}.${SSODOMAIN}\n\
 
        ErrorLog /var/log/apache2/proxysite_error.log\n\
        CustomLog /var/log/apache2/proxysite_access.log combined\n\
</VirtualHost>"\
>> /etc/apache2/sites-available/${SERVICE_NAME}-apache2.conf

# Enable sites
RUN a2ensite handler-apache2.conf
RUN a2ensite portal-apache2.conf
RUN a2ensite manager-apache2.conf
RUN a2ensite test-apache2.conf
RUN a2ensite ${SERVICE_NAME}-apache2.conf

# Remove cached configuration
RUN rm -rf /tmp/lemonldap-ng-config

EXPOSE 80 443
VOLUME ["/var/log/apache2", "/etc/apache2", "/etc/lemonldap-ng", "/var/lib/lemonldap-ng/conf", "/var/lib/lemonldap-ng/sessions", "/var/lib/lemonldap-ng/psessions"]
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
