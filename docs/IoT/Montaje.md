---
layout: page
title: Montaje componentes estación
nav_order: 4
---

# Montaje componentes estación
 

La instalación del circuito es muy sencilla. El NodeMCU trabaja con voltajes de 3,3v (a diferencia de Arduino que trabaja a 5v). Según el datasheet del DHT22, este sensor trabaja entre voltajes de 3,3V y 5,5V. Mi modelo de sensor sólo tiene 3 patillas, una de alimentación, otra de tierra (GND) y la de datos. No es necesaria ninguna resistencia intermedia y lo podemos conectar directamente a la placa.

El pin de alimentación (+) del sensor lo conectamos a uno de salida de 3,3V del NodeMCU, el de GND (-) a uno de GND, y por último, el pin central de datos lo conectamos a un pin GPIO de la placa. En mi caso lo he conectado al pin D2.

Es importante elegir bien el pin para la programación posterior de la placa.

 

Aunque la placa trabaja a 3,3V, para alimentarla podemos utilizar un cargador microUSB de 5V, como los de cualquier móvil. La propia placa tiene un regulador de tensión en elpuerto USB que la baja a 3,3V.
