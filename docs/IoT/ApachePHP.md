---
layout: page
title: Servidor Web y PHP
nav_order: 9
---
# Servidor Web y PHP

El último servicio que debemos instalar es el del servidor Web que nos proporcionará los datos cuando nos conectemos remotamente desde cualquier navegador.

Vamos a instalar el servidor web Apache, y también lo vamos a ejecutar en un contenedor de Docker.

Comenzamos creando el directorio donde almacenaremos nuestra página web en una carpeta local de la Raspberry Pi.

    $ mkdir ~/estacion/htdocs

Instalamos y corremos el contenedor:

    $ docker run --name apache -d -p 80:80 -v /home/pi/estacion/htdocs:/var/www/html php:apache

Instalamos la extensión de PHP para acceder a base de datos MySQL. Para ello ejecutamos 3 comandos dentro del contenedor. Lo podemos hacer desde fueracon la función exec de Docker:

    $ docker exec -it apache docker-php-ext-install mysqli
    $ docker exec -it apache docker-php-ext-enable mysqli
    $ docker exec -it apache apachectl restart

Ya tenemos nuestro servidor web en funcionamiento.
