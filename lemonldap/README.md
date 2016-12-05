```
docker build Base/. -t lemon/base  

docker build SP/. -t lemon/sp  

docker build IdP/. -t lemon/idp  

docker build Mellon/. -t lemon/mellon  

docker network create inside  

docker run -d --network inside --network-alias tomcat.fr --name tomcat tomcat   

docker run -ti --name sp --add-host reload.lemon.sp.fr:127.0.0.1 -p  192.168.99.100:80:80 --network inside lemon/sp  

docker network create outside  

docker run -ti --name idp --add-host reload.lemon.idp.fr:127.0.0.1 -p 192.168.56.101:80:80 --network outside  --network-alias auth.lemon.idp.fr lemon/idp  

docker network connect outside sp

docker pull dockerage/apache-mellon

docker run -ti --name mellon -p 192.168.99.100:80:80 --network inside lemon/mellon

docker network connect outside mellon

```
