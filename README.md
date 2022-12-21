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
----------------------------------------------------------\
if nginx docker is stop and while starting it errors 443 and 80 ports are already in use :
sudo lsof -nP | grep LISTEN
kill ID
restart docker nginx again
---------------------------------

php.ini location in docker phpmyadmin
# First log into the running container
$ docker exec -it «container_name» /bin/bash

# List folder content
$ ls /usr/local/etc/php


conf.d  php.ini-development  php.ini-production

If needed, modifying settings via conf.d folder seems better alternative, since xdebug uses it. For example, you can change upload size by adding uploads.ini to conf.d folder with the following content:

file_uploads = On
memory_limit = 64M
upload_max_filesize = 64M
post_max_size = 64M
max_execution_time = 600

------------------------------------
The error “413 – Request Entity Too Large” indicates that web server configured to restrict large file size.
sudo nano /etc/nginx/nginx.conf
# set client body size to 2M #
client_max_body_size 2M;
------------------------------------

