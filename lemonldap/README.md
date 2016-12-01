docker build Base/. -t lemon/base  

docker build SP/. -t lemon/sp  

docker build IdP/. -t lemon/idp  

docker network create inside  

docker run -ti --network inside  --network-alias tomcat.fr --name tomcat --rm tomcat   

docker run -ti --name sp --add-host reload.lemon.sp.fr:127.0.0.1 -p 80:80 --network inside lemon/sp  

docker network create outside  

docker run -ti --name idp --add-host reload.lemon.idp.fr:127.0.0.1 -p 81:80 --network outside lemon/idp  

docker network connect outside sp  

