# But 

Test SSO SAML avec *mod_auth_mellon* et *LemonLDAP NG*

# Installation

```
docker build Base/. -t lemon/base  

docker build SP/. -t lemon/sp  

docker build IdP/. -t lemon/idp  

docker build Mellon/. -t lemon/mellon  

docker network create outside
docker network create border  
docker network create inside

docker run -d --network inside --network-alias tomcat.fr --name tomcat tomcat  

docker run -d --name mellon -p 192.168.19.100:80:80 --network inside lemon/mellon 

docker run -d --name sp --add-host reload.lemon.sp.fr:127.0.0.1 -p 192.168.99.100:80:80 --network border  --network-alias auth.lemon.sp.fr lemon/sp  

docker network connect outside sp
  
docker run -d --name idp --add-host reload.lemon.idp.fr:127.0.0.1 -p 192.168.56.101:80:80 --network outside --network-alias auth.lemon.idp.fr lemon/idp  

docker build WSO2IS/. -t wso2is

docker run -d --name wso2is -p 192.168.56.101:9443:9443 -p 192.168.56.101:9763:9763 -p 192.168.56.101:8000:8000 -p 192.168.56.101:10500:10500  --network outside --network-alias wso2is.idp.fr wso2is

docker build Kerberos/. -t kerberos 
```

# Configuration

* Ajouter les metadata du sp à mellon
* Configuration de l'authentification double dans le SP : Demo et SAML
* Création du Service SAML dans le SP :
  * Configuration Service SAML
  * Déclaration comme fournisseur d'identité
  * Ajout de mellon comme fournisseur de service avec les metadata mellon
  * Ajout de l'IdP comme fournisseur d'identité avec ses metadata
* Création du Service SAML dans l'IdP :
  * Configuration Service SAML
  * Déclaration comme fournisseur d'identité
  * Ajout du SP comme fournisseur de service avec ses metadata
  

