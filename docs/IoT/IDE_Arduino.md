---
layout: page
title: Preparación entorno IDE Arduino
nav_order: 3
---
# Preparación entorno IDE Arduino

Para programar la tarjeta NodeMCU vamos a utilizar el IDE de Arduino, adaptándolo a nuestra tarjeta.

El primer paso, por tanto, es descargar el entorno de desarrollo desde la web oficial de Arduino. Accedemos a https://www.arduino.cc/en/Main/Software y descargamos el IDE.

Una vez descargado e instalado, hay que configurarlo para poder trabajar con NodeMCU. Para ello hemos de indicar que la tarjeta con la que queremos trabajar es la NodeMCU 1.0.

Por defecto, el soporte para esta tarjeta no viene instalado, así que hay que instalar el paquete manualmente.

Para ello, en el desplegable, seleccionamos Gestor de Tarjetas:

En el campo de búsqueda tecleamos ESP8266:

Si nos aparece, basta con darle a Instalar, pero la primera vez no nos aparecerá y por tanto hay que cambiar las preferencias para que pueda acceder a los repositorios y descargar el soporte para esta tarjeta.

Por tanto, previamente accedemos a las Preferencias del IDE de Arduino y en el campo Gestor de URLs adicionales de Tarjetas introducimos la siguiente URL:

https://arduino.esp8266.com/stable/package_esp8266com_index.json

Una vez hecho ese paso ya podremos buscar e instalar el gestor de tarjetas para ESP8266.

Por último, sólo hay que seleccionar la tarjeta NodeMCU 1.0

