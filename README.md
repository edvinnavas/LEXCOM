# LEXCOM
Servicio Rest API capa lógica Lexcom.
Jersey Rest API.

sudo docker run -d --name LEXCOM-PORTAINER -p 8000:8000 -p 9443:9443 -p 9000:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v "/home/edvin_navas/VolumenDocker/Portainer":/data --env TZ=America/Guatemala portainer/portainer-ce:latest

sudo docker network create lexcom_network

sudo docker run -d --name LEXCOM-MYSQL --network lexcom_network -p 3306:3306 --restart=always --env MYSQL_ROOT_PASSWORD=L3xc0m@dm1n --env TZ=America/Guatemala --volume "/home/edvin_navas/VolumenDocker/LexcomMysql":/var/lib/mysql mysql:latest

mysql> CREATE DATABASE db_lexcom;
mysql> CREATE USER 'user_lexcom'@'%' IDENTIFIED BY 'L3xc0m@dm1n';
mysql> GRANT ALL ON db_lexcom.* TO 'user_lexcom'@'%';
mysql> FLUSH PRIVILEGES;

docker run -d --name LEXCOM-PAYARA --network lexcom_network -p 9001:8080 -p 4848:4848 --restart=always --env TZ=America/Guatemala payara/server-full:6.2023.6

docker build -t unocorp/transportes-cliente-rest-api:1.0.0 .
docker run -p 9002:8018 -t -i --network unocorp_network --name UNOCORP-CLIENTE-REST-VIAJES --restart=always --env TZ=America/Guatemala unocorp/transportes-cliente-rest-api:1.0.0

docker build -t unocorp/unocorp-rest-api:1.0.0 .
docker run -p 9003:8080 -t -i --network unocorp_network --name UNOCORP-REST-API --restart=always --env TZ=America/Guatemala unocorp/unocorp-rest-api:1.0.0

docker build -t unocorp/unocorp-cliente-rest-sms:1.0.0 .
docker run -p 9004:8080 -t -i --network unocorp_network --name UNOCORP-REST-API-SMS --restart=always --env TZ=America/Guatemala unocorp/unocorp-cliente-rest-sms:1.0.0