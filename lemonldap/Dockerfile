# Dockerfile for LemonLDAP::NG
# Use debian repo of LemonLDAP::NG project
# Adapted from oudot

# Start from Debian Jessie
FROM coudot/lemonldap-ng
MAINTAINER Tanguy Le Berre

# Change SSO DOMAIN here
ENV SSODOMAIN lemon.sp.fr

# Change SSO Domain
RUN sed -i "s/example\.com/${SSODOMAIN}/g" /etc/lemonldap-ng/* /var/lib/lemonldap-ng/conf/lmConf-1.js /var/lib/lemonldap-ng/test/index.pl

ENTRYPOINT []
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
