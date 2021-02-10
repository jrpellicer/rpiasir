---
layout: page
title: Docker Swarm
---
# Cluster de Docker Swarm

Esta práctica consiste en la creación de un Cluster de Raspberrys con el fin de ejecutar contenedores en el mismo. El hecho de utilizar un cluster nos va a proporcionar ventajas como **Alta Disponibilidad** o **Escalabilidad** en nuestro sistema, de modo que el servicio que estemos ejecutando en un contenedor esté siempre disponible aunque caiga alguno de los nodos del cluster, y que además sea sencillo de escalar (ampliar) la capacidad de respuesta del servicio para satisfacer necesidades.

Para poder crear un cluster necesitaremos de varias Raspberrys, por tanto esta práctica debe hacerse por grupos (3 es un número ideal) compartiendo las Raspberrys. Los ejemplos que se detallan a continuación son para un cluster formado por 3 RPi, 1 de ellas actuando como servidor maestro (*manager* en el argot de Docker Swarm) y las otras 2 como esclavos (*workers* en Docker Swarm).


## Docker Swarm


