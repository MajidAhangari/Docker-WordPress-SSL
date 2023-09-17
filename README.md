

 Docker-WordPress-SSL
WordPress + MySQL + phpMyAdmin + Nginx + SSL - CMS - Docker-compose

phpMyAdmin "localhost:8080"

in docker file modify

VIRTUAL_HOST: tudominio.com        insert your site name
LETSENCRYPT_HOST: tudominio.c      insert your site name


in order to disable auto login for Phpmyadmin, you should remove PMA_USER and PMA_PASSWORD, just keep PMA_HOSTS,
then the phpmyadmin will ask for login to access the database.

if nginx docker is stop and while starting it errors 443 and 80 ports are already in use :
sudo lsof -nP | grep LISTEN
kill ID
restart docker nginx again



php.ini location in docker phpmyadmin
 First log into the running container
$ docker exec -it «container_name» /bin/bash

 List folder content
$ ls /usr/local/etc/php


conf.d  php.ini-development  php.ini-production

If needed, modifying settings via conf.d folder seems better alternative, since xdebug uses it. For example, you can change upload size by adding uploads.ini to conf.d folder with the following content:

file_uploads = On
memory_limit = 64M
upload_max_filesize = 64M
post_max_size = 64M
max_execution_time = 600


The error “413 – Request Entity Too Large” indicates that web server configured to restrict large file size.
sudo nano /etc/nginx/nginx.conf
 set client body size to 64M 
client_max_body_size 64M;
And then install plugin "Big File Uploads"

*****************************IMPORTANT********************
The "413 Request Entity Too Large" error in Nginx indicates that the server is rejecting a request because the request's body size exceeds the maximum allowed size configured in Nginx. This issue is often encountered when you're trying to upload a file or submit a form with a large amount of data.

To resolve this issue in a Dockerized WordPress environment, you'll need to adjust the Nginx configuration within your Nginx container. Here are the steps to do that:

Access the Nginx container as explained in the previous response:

nginx-proxy

docker exec -it <nginx_container_name_or_id> /bin/bash

Navigate to the Nginx configuration directory, which typically includes a nginx.conf file or a conf.d directory:

cd /etc/nginx

Edit the Nginx configuration file. You can use a text editor like nano or vim:


nano nginx.conf

Inside the nginx.conf file or a relevant configuration file (e.g., conf.d/wordpress.conf), you should add or modify the following directive to increase the maximum allowed request body size. This directive is typically found within an http, server, or location block:

client_max_body_size 20M; # Change this value to your desired limit (e.g., 20M for 20 megabytes)

service nginx restart

exit

Now, Nginx should allow larger request bodies, and you shouldn't encounter the "413 Request Entity Too Large" error when uploading files or submitting forms in your Dockerized WordPress environment. Remember to replace <nginx_container_name_or_id> with the actual name or ID of your Nginx container.





*****************************IMPORTANT*************************

If you're encountering an error stating that "The uploaded file exceeds the upload_max_filesize directive in php.ini" in a Dockerized WordPress environment, you need to increase the upload_max_filesize and post_max_size directives in the PHP configuration. Here's how you can do it:

Access the WordPress container:

docker exec -it <wordpress_container_name_or_id> /bin/bash

Replace <wordpress_container_name_or_id> with the actual name or ID of your WordPress container.

Navigate to the PHP configuration directory. Depending on your PHP version and setup, the PHP configuration file may be named php.ini, php.ini-development, or php.ini-production. Common locations include /usr/local/etc/php/php.ini or /etc/php/7.x/php.ini, where 7.x is the PHP version you are using. Use the find command to locate the file if needed:

find /etc -name php.ini

Edit the PHP configuration file using a text editor like nano or vim:


nano /path/to/php.ini

Replace /path/to/php.ini with the actual path to your PHP configuration file.

Inside the PHP configuration file, locate the following lines:

upload_max_filesize = 2M
post_max_size = 8M

Change the values to increase the upload_max_filesize and post_max_size directives to a value that suits your needs. For example:
upload_max_filesize = 64M
post_max_size = 64M

Be sure to use the same value for both directives.

Save the changes and exit the text editor.

Restart the PHP service within the WordPress container to apply the changes:

service php7.4-fpm restart  # Use the appropriate PHP version (e.g., php7.4-fpm)

Exit the WordPress container:





The uploaded file exceeds the upload_max_filesize directive in php.ini.



********************JUST DO THIS**IMPORTAND******************

in wordpress container cd /usr/local/etc/php/  make a file name php.ini 
nano php.ini
and put this in it
upload_max_filesize = 64M



