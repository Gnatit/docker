FROM debian:jessie
MAINTAINER dockerage <dockerage@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV DEBIAN_PRIORITY=critical

RUN apt-get update && apt-get -y install \
	apache2 \
	pkg-config \
	libcurl3 \
	libssl1.0.0 \
	liblasso3 \
	libapache2-mod-auth-mellon \
	vim \
	ack-grep
	

# Activate mod_proxy
RUN a2enmod \
	rewrite \
	headers \
	proxy \
	proxy_http \
	proxy_wstunnel

# Set to debug
RUN sed -i "s/LogLevel warn/LogLevel debug/g" /etc/apache2/apache2.conf

# Generate Mellon metadata
ADD ./mellon	/etc/apache2/mellon
WORKDIR /etc/apache2/mellon_conf/
RUN chmod +x /etc/apache2/mellon/mellon-create-metadata.sh
RUN /etc/apache2/mellon/mellon-create-metadata.sh  http://mellon.sp.fr/sp http://mellon.sp.fr/mellon

# Configure mellon
RUN echo '# This is a server-wide configuration that will add information from the Mellon session to all requests.\n\
<Location />\n\
    # Add information from the mod_auth_mellon session to the request.\n\
    MellonEnable "info"\n\
\n\
    # Configure the SP metadata\n\
    # This should be the files which were created when creating SP metadata.\n\
    MellonSPPrivateKeyFile /etc/apache2/mellon_conf/http_mellon.sp.fr_sp.key\n\
\n\
    MellonSPCertFile /etc/apache2/mellon_conf/http_mellon.sp.fr_sp.cert\n\
    MellonSPMetadataFile /etc/apache2/mellon_conf/http_mellon.sp.fr_sp.xml\n\
\n\
    # IdP metadata. This should be the metadata file you got from the IdP.\n\
    MellonIdPMetadataFile /etc/apache2/mellon_conf/idp-metadata.xml\n\
\n\
    # The location all endpoints should be located under.\n\
    # It is the URL to this location that is used as the second parameter to the metadata generation script.\n\
    # This path is relative to the root of the web server.\n\
    MellonEndpointPath /mellon\n\
</Location>'\
>> /etc/apache2/sites-available/000-default.conf

RUN echo '<VirtualHost *:80>\n\
	ServerName tomcat.mellon.sp.fr\n\
	\n\
	<Location />\n\
		MellonEnable "auth"	\n\
	</Location>\n\
\n\
	ProxyPass / http://tomcat.fr:8080/\n\
	ProxyPassReverse / http://tomcat.fr:8080/\n\
	ProxyPassReverseCookieDomain tomcat.fr tomcat.mellon.sp.fr\n\
	ErrorLog /var/log/apache2/proxysite_error.log\n\
	CustomLog /var/log/apache2/proxysite_access.log combined\n\
</VirtualHost>'\
>> /etc/apache2/sites-available/tomcat-apache2.conf

# Enable site
RUN a2ensite tomcat-apache2.conf

# File to safely remove pid
RUN echo '#!/bin/bash\n\
rm  /var/run/apache2/apache2.pid\n\
/usr/sbin/apache2ctl -D FOREGROUND'\
>> /usr/bin/safe_apache

RUN chmod +x /usr/bin/safe_apache

EXPOSE 80 443
ENTRYPOINT ["/usr/bin/safe_apache"]
