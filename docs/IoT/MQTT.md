---
layout: page
title: Instalación Broker MQTT
nav_order: 6
---
# Instalación Broker MQTT en Raspberry Pi
 
MQTT (Message Queue Telemetry Transport) es un protocolo ligero de red muy utilizado en el mundo del IoT. Está basado en un protocolo de mensajería publicación/suscripción. La gran ventaja de este protocolo es que es muy simple y ligero, ideal para sistemas poco potentes, con poco ancho de banda y dispositivos que requieren de poco consumo.

MQTT tiene una arquitectura en estrella, de manera que hay un nodo principal, el servidor, y muchos nodos clientes conectados a él.

Al nodo servidor, en MQTT, se le denomina Broker. El Broker es la parte central de la arquitectura y todos los mensajes pasan por él, se encarga de distribuir los mensajes a los receptores.

Los clientes envían mensajes al Broker, y éste reenvía el mensaje a los receptores que se hayan suscrito a una publicación concreta.

El topic es el tema donde se suscriben los receptores para recibir el mensaje. Lo podemos asemejar como el asunto de un email. El mensaje no tiene destinatario concreto, puede haber uno, muchos o ninguno. Aquellos clientes que estén suscritos al topic, recibirán el mensaje.

 

En nuestro proyecto, el NodeMCU leerá los datos del sensor de temperatura y humedad y los pasará por MQTT al Broker que instalaremos en la Raspberry.

El Topic que utilizaremos será, por ejemplo:

    casa/estacion/sensor1

Y en el mismo mensaje pasaremos la temperatura y la humedad registrada:

    casa/estacion/sensor1/"22.1,87.3"

El Broker que vamos a instalar en la Raspberry es el Mosquitto. Puesto que existe en el Hub de Docker una versión válida para Raspberry, la utilizamos.

    $ docker run --name mosquitto -it -p 1883:1883 -p 9001:9001 eclipse-mosquitto

Si se nos queda en primer plano, podemos detenerlo con CTRL+C y luego volver a ejecutarlo con el comando start:

    $ docker start mosquitto
 




