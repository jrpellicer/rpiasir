---
layout: page
title: Instalación Node-Red
nav_order: 8
---
# Instalación Node-Red
 

Node-RED es una herramienta de programación visual muy potente que sirve para comunicar hardware y servicios de una forma muy rápida y sencilla. Nos permite conectar dispositivos hardware, APIs y servicios en línea mediante un editor basado en navegador que facilita la conexión de los flujos con unos pocos clics. Su punto fuerte es la sencillez. Nos permite utilizar tecnologías complejas sin tener que profundizar hasta el más mínimo detalle en todas ellas. Nos quedamos en una capa inicial donde nos centramos en lo importante y dejamos de lado aquello que no es práctico, y todo con una herramienta web, de forma visual y sin apenas programar.

La estructura mínima son los nodos. Estos se arrastran a través de la interfaz gráfica y nos permiten hacer una tarea concreta. Recibir una llamada HTTP, un mensaje MQTT o la activación de un pulsador.

Todos estos nodos se organizan en flujos o flows que agrupan nodos que se conectan entre ellos.

En nuestro proyecto vamos a trabajar con un nodo que será la recepción del mensaje MQTT, el resultado se le pasará a otro nodo que extraerá la información, la procesará y la enviará a otro nodo que generará una consulta SQL que finalmente atacará sobre un último nodo que escribirá la información en una BBDD MySQL.

Comenzamos con la instalación de Node Red en nuestra Raspberry Pi. Vamos a utilizar también un contenedor para su instalación.

Creamos un directorio de trabajo para el almacenamiento persistente de los datos de contenedor:

    $ mkdir estacion/node-red –p

Ejecutamos el contenedor:

    $ docker run -d -p 1880:1880 --name nodered -v /home/pi/estacion/node-red:/data nodered/node-red

Si se nos queda en primer plano, podemos detenerlo con CTRL+C y luego volver a ejecutarlo con el comando start:

    $ docker start nodered

Para acceder al entorno de trabajo, abrimos un navegador y accedemos a la dirección de la Raspberry por el puerto 1880

En primer lugar vamos a instalar una extensión para poder tener un nodo que nos permita leer y escribir en una base de datos MySQL. Para ello accedemos al menú (icono de las 3 líneas horizontales en la parte superior derecha de la ventana) y accedemos a la opción Settings y dentro de ella, a la pestaña Palette. Buscamos en el recuadro la palabra mysql e instalamos la librería.

Una vez instalada, procedemos a la creación de nuestro flujo. Para ello comenzamos poniendo 4 nodos de los siguientes tipos:
- mqtt in
- csv
- function
- mysql

Los unimos mediante un flujo arrastrando el conector gris de un nodo hacia el siguiente:

Cada nodo enviará un mensaje (msg) al siguiente nodo con un encabezado (msg.topic) y un cuerpo (msg.payload). Cada nodo que reciba el mensaje lo procesará de un modo (según el tipo de nodo) y reenviará un nuevo mensaje al siguiente nodo.

Hacemos doble clic sobre ellos y comenzamos a configurarlos.

Comenzamos por el nodo Mqtt in. Introducimos los datos de nuestro Broker (la dirección IP de la Raspberry y el puerto 1883 por defecto) y el topic al que deseamos suscribirnos. En nuestro caso nos suscribimos al topic casa/estacion/# donde el símbolo # indica que estamos suscritos a los subtopics que cuelguen de casa/estacion, dejando así preparada la lectura de los mensajes enviados por varios clientes mqtt que utilicen el topic casa/estacion:

En el nodo csv configuramos para decir que el mensaje recibido son 2 campos separados por comas que corresponden a la Temperatura y Humedad que envió el sensor.

En el siguiente nodo creamos una función a partir de los datos recibidos para que el mensaje que le pase al siguiente nodo tenga una instrucción SQL en su cabecera. Esta instrucción SQL será la que insertará en la BBDD la fila con los datos leídos:

Tecleamos la siguiente función en javascript:

```js
// Extraemos la Temperatura y Humedad del cuerpo del mensaje creado en el nodo CSV
var temp=msg.payload.Temperatura;
var hum=msg.payload.Humedad;

// Extraemos el código del sensor que viene en el TOPIC y se arrastra desde el primer nodo
var n = msg.topic.lastIndexOf("/");
var sensor = msg.topic.substr(n+1);

// Modificamos el asunto del mensaje para que lo reciba el siguiente nodo y en el que aparezca una sentencia SQL
msg.topic = "INSERT INTO mediciones (sensor,temperatura,humedad) VALUES ('"+sensor+"',"+temp+","+hum+")";

return msg;
```

Por último, editamos el último nodo con la BBDD Mysql. Asignamos un nombre y configuramos la conexión:

Una vez creados y configurados los nodos, sólo nos queda pulsar sobre el botón Deploy. Si todo ha ido bien, ya está en marcha nuestro flujo de trabajo esperando a recibir un mensaje mqtt de nuestro Broker para procesarlo y almacenarlo en la BBDD. Con unos pocos clics y una pequeña función de javascript nos hemos olvidado de tener que programar eventos, configurar protocolos, servicios, instalar librerías de programación, etc.

