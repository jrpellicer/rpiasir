---
layout: page
title: Instalación MySQL
nav_order: 7
---
# Instalación MySQL

Como deseamos almacenar todas las lecturas del sensor en una base de datos, vamos a proceder a la instalación del gestor de base de datos MySQL. Optamos también por instalarlo en un contenedor de Docker.

En primer lugar creamos el directorio necesario para que se almacenen los datos en la raspberry:

    $ mkdir ~/estacion/mysql/data -p

Creamos y ejecutamos el contenedor especificando que se monte un volumen para almacenar los datos en el directorio que acabamos de crear:

    $ docker run --name mysql -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=P@ssw0rd -v /home/pi/estacion/mysql/data:/var/lib/mysql hypriot/rpi-mysql

Una vez está en ejecución, vamos a abrir un terminal para crear la base de datos y la tabla:

    $ docker exec -it mysql mysql -p

Y tecleamos los siguientes mandatos:

    create database meteo;
    use meteo;
    CREATE TABLE IF NOT EXISTS `mediciones` (
    `fecha` TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT 'Fecha y hora de la lectura',
    `sensor` varchar(50) NOT NULL COMMENT 'Nombre del sensor',
    `temperatura` float COMMENT 'Temperatura',
    `humedad` float COMMENT 'Humedad',
    PRIMARY KEY (`fecha`,`sensor`),
    KEY `sensor` (`sensor`)
    ) COMMENT='Mediciones de temperatura';
    exit
 
