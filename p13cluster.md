---
layout: page
title: Cluster
---
# Estación Meteorológica

Proyecto de estación meteorológica con NodeMCU, sensor DHT22 y Raspberry Pi

## Objetivo del proyecto

Creación de una estación meteorológica conectada a la red con la posibilidad de acceder a sus datos remotamente vía web. Para ello, se implementa un pequeño sistema IoT doméstico con sensores de temperatura y humedad conectados por wifi, que almacena las lecturas obtenidas en una base de datos ubicada en una Raspberry Pi y que a su vez también actúa de servidor web.

El paso de los datos desde el sensor hasta la Raspberry Pi se hace mediante el protocolo MQTT. Los mensajes MQTT recibidos vía WiFi en la Raspberry se procesan mediante Node-RED y se almacenan en una BBDD MySQL, estando disponibles para ser accedidos por el servidor web Apache también ubicado en la Raspberry Pi.

Todos los servicios aojados en la Raspberry Pi están contenerizados con Docker.
