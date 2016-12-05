# Dockerfile for mod_auth_mellon
# Adapted from work by dockerage
# https://github.com/dockerage/dockers

# Start from Debian Jessie
FROM dockerage/apache-mellon
MAINTAINER Tanguy Le Berre

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



 ./mellon-create-metadata.sh  http://mellon.sp.fr/sp http://mellon.sp.fr/mellon
 
 # This is a server-wide configuration that will add information from the Mellon session to all requests.
<Location />
    # Add information from the mod_auth_mellon session to the request.
    MellonEnable "info"

    # Configure the SP metadata
    # This should be the files which were created when creating SP metadata.
    MellonSPPrivateKeyFile /etc/apache2/mellon/http_mellon.sp.fr_sp.key

    MellonSPCertFile /etc/apache2/mellon/http_mellon.sp.fr_sp.cert
    MellonSPMetadataFile /etc/apache2/mellon/http_mellon.sp.fr_sp.xml

    # IdP metadata. This should be the metadata file you got from the IdP.
    MellonIdPMetadataFile /etc/apache2/mellon/idp-metadata.xml

    # The location all endpoints should be located under.
    # It is the URL to this location that is used as the second parameter to the metadata generation script.
    # This path is relative to the root of the web server.
    MellonEndpointPath /mellon
</Location>

<VirtualHost *:80>
        ServerName tomcat.mellon.sp.fr
		
		<Location />
			MellonEnable "auth"	
		</Location>
    
        ProxyPass / http://tomcat.fr:8080/
        ProxyPassReverse / http://tomcat.fr:8080/
        ProxyPassReverseCookieDomain tomcat.fr tomcat.mellon.sp.fr
        ErrorLog /var/log/apache2/proxysite_error.log
        CustomLog /var/log/apache2/proxysite_access.log combined
</VirtualHost>