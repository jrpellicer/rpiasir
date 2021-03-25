---
layout: page
title: Preparación Raspberry Pi
nav_order: 5
---

# Preparación Raspberry Pi

Para trabajar con la Raspberry Pi es necesario tener instalado y correctamente configurado Raspbian, con la zona horaria correspondiente.

Es recomendable tener asignada una IP fija a la Raspberry. Podemos hacerlo mediante la configuración del adaptador de red en el propio sistema operativo o mediante la configuración de nuestro servidor DHCP en el router para que siempre le asigna la misma dirección IP dinámica.

En este ejemplo, la dirección que se va a utilizar para todos los servicios alojados en la Raspberry es la dirección local 192.168.1.27.

Puesto que todos los servicios que necesitamos en la Raspberry (Mosquitto, Node-RED, MySQL y Apache) los vamos a tener en contenedores, es necesario que instalemos Docker previamente.

Abrimos sesión con root:

    $ sudo su

El proceso comienza descargando una serie de paquetes necesarios:

    # apt update
    # apt install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common

A continuación obtenemos la clave firmada para la descarga de los paquetes de Docker:

    # curl -fsSL https://download.docker.com/linux/raspbian/gpg | sudo apt-key add –


Añadimos el repositorio oficial de Docker a nuestra lista de repositorios (aquí ponemos de manera automática la release que estamos utiliozando y de manera manual la arquitectura de la raspberry):

    # echo "deb [arch=armhf] https://download.docker.com/linux/raspbian $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list

Procedemos con la instalación de Docker:

    # apt update
    # apt install docker-ce

Si deseamos ejecutar Docker con un usuario sin privilegios debemos añadir nuestro usuario al grupo docker. En mi caso se trata del usuario pi y lo hacemos con el lanzamiento del siguiente comando:

    # usermod -aG docker pi

Sólo queda activar para que se ejecute en el arranque e iniciar el servicio:

    # systemctl enable docker
    # systemctl start docker

Será necesario cerrar sesión y volver a entrar para que el usuario tenga privilegios.