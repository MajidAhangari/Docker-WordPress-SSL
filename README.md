# Docker-WordPress-SSL
WordPress + MySQL + phpMyAdmin + Nginx + SSL - CMS - Docker-compose

Ver en "localhost:80"

phpMyAdmin - ver en "localhost:8080"

Modificar las líneas 23 y 24 de docker-compose.yaml:

23  VIRTUAL_HOST: tudominio.com        #debe ser un dominio válido y DNS apuntando a este servidor
24  LETSENCRYPT_HOST: tudominio.com    #debe ser un dominio válido y DNS apuntando a este servidor   


********************************************************************************
in docker file modify

VIRTUAL_HOST: tudominio.com        #insert your site name
LETSENCRYPT_HOST: tudominio.c      #insert your site name

************************************************************


In order to disable auto login for Phpmyadmin, you should remove PMA_USER and PMA_PASSWORD, just keep PMA_HOSTS,
then the phpmyadmin will ask for login to access the database.
