---
layout: page
title: Cluster
nav_order: 15
---
# Cluster de Computación Paralela

## Computación Paralela
Hemos visto en el proyecto anterior cómo unir varias Raspberry Pi para crear un cluster con el que conseguir una alta disponibilidad y aumentar la escalabilidad de un modo sencillo utilizando servicios ejecutándose en contenedores. En ese caso anterior hemos utilizado un orquestador de contenedores (el Docker Swarm) que nos ha facilitado enormemente la tarea de repartir los procesos entre los nodos.

Otro de los usos de los clusters es el de la computación paralela, muy utilizado en supercomputación.

Los supercomputadores no son más que un conjunto de poderosos ordenadores unidos entre sí para aumentar su potencia de trabajo y rendimiento. Se usan para tareas de cálculos intensivos, tales como problemas que involucran física cuántica, predicción del clima, investigación de cambio climático, modelado de moléculas, simulaciones físicas tal como la simulación de aviones o automóviles en el viento, simulación de la detonación de armas nucleares, investigación en la fusión nuclear, etc.

Según el listado del TOP500 de Noviembre de 2020, el supercomputador más potente del mundo es el japonés Fugaku que está formado por 158.976 procesadores ARM A64FX de 48 núcleos cada uno, que nos dan un total de 7.630.848 cores.

El supercomputador más potente que tenemos en Valencia es el Tirant, formado por 336 nodos que tienen cada uno 2 procesadores Intel Xeon de 8 cores cada uno, dando un total de 5.376 núcleos, muy muy lejos del Fugaku.

En esta práctica vamos a formar un cluster de 3 Raspberry Pi, que con los 4 núcleos que tiene cada procesador ARM nos dará un mini-supercomputador de 12 cores.

Una de las técnicas que se utilizan en supercomputación para ejecutar procesos en paralelo es la de programación mediante librerías MPI (Interfaz de Paso de Mensajes) que permiten la ejecución de programas en múltiples nodos de forma paralela con el paso de mensajes entre los nodos.

Nosotros vamos a ejecutar de forma paralela un pequeño programa en Python que utiliza esta librería y comprobaremos los tiempos de respuesta dependiendo del número de procesos que lancemos concurrentemente.

## Preparación de los nodos
Vamos a montar un cluster con 3 Raspberry Pi. No es necesario hacer nada especial. Únicamente han de estar las 3 Raspberry Pi en la misma red y asegurarnos que se vean. Una de las Raspberry la elegiremos como nodo maestro.

En primer lugar vamos a nombrar cada nodo con un nombre único. Podemos utilizar como nombres *Maestro*, *Nodo1* y *Nodo2*. Si los nombres ya existieran en la red (de otros compañeros) utilizaremos otra combinación. Podemos utilizar los nombres que usamos en la práctica anterior o ejecutar el comando de configuración para cambiar los nombres en cada uno de los nodos:

    $ sudo raspi-config

### Instalación de las librerías necesarias
Para poder ejecutar los programas de demo en Python de esta práctica es necesario instalar las librerías *mpi4py* y *numpy*.

La primera es la que realmente va a hacer posible la ejecución de los programas en paralelo. El paquete nos instala las librerías de python y también las utilidades necesarias para la ejecución.

    $ sudo apt-get update
    $ sudo apt-get install -y python-mpi4py python-numpy

**La instalación debemos hacerla en el maestro y en todos los nodos esclavos del cluster.**

### Instalación de Git y descarga del repositorio de programas
El siguiente paso será descargarnos los programas en Python de prueba. Puesto que están en un repositorio de Git, vamos a instalar en primer lugar el programa de gestión de versiones Git.

    $ sudo apt-get install git

Descargamos el repositorio:

    $ git clone https://github.com/jrpellicer/mpi4py.git

Comprobamos con un ls que tenemos el directorio creado y los contenidos descargados:

    $ ls ./mpi4py/
    helloworld.py  LICENSE  primos.py  README.md

**Este paso también debemos hacerlo en el nodo maestro y esclavos.**

### Instalación y configuración de SSH
El siguiente paso sería instalar el paquete *ssh-server*, pero no es necesario puesto que ya lo tenemos instalado, de lo contrario no podríamos estar conectados por SSH a cada una de las Raspberrys.

Lo que sí que vamos a hacer es crear una clave pública en cada uno de los nodos. Esto sólo es necesario hacerlo en el nodo maestro, pero lo vamos a hacer en todos los nodos (también en el maestro) para que se cree automáticamente el directorio *~/.ssh* en todos los nodos:

    $ ssh-keygen
    (pulsamos Intro en cada una de las opciones que nos pide)

## Preparación del Nodo Maestro
Estos pasos los vamos a realizar **únicamente en el nodo maestro**. En los nodos esclavos no hace falta hacer nada.

### Creación del machinefile
Para indicarle a MPI qué nodos componen el cluster debemos crear un fichero especificando las direcciones IP de cada uno de los nodos (incluido el maestro).

El fichero lo vamos a llamar *machinefile* y lo vamos a crear en la carpeta personal del usuario *pi*. Ojo, sólo en el nodo maestro:

    pi@Maestro:~ $ nano machinefile

Un contenido de ejemplo de este fichero sería:

    192.168.1.11
    192.168.1.14
    192.168.1.15

Donde en cada línea hemos introducido la IP de cada nodo del cluster (incluido el maestro).

### Copia de la clave pública en nodos
Puesto que los programas en paralelo se van a lanzar desde el nodo maestro, es necesario que desde este nodo se establezca una conexión ssh automática a los esclavos. Para ello vamos a copiar la clave pública del usuario *Pi* que hemos creado en el maestro a cada uno de los nodos. Esto lo hacemos copiando esa clave pública al fichero *~/.ssh/authorized_keys* de cada nodo esclavo.

Para la copia ejecutamos desde el nodo maestro, especificando la IP del Nodo1, el siguiente comando:

    pi@Maestro:~ $ cat ~/.ssh/id_rsa.pub | ssh pi@192.168.1.14 "cat >> .ssh/authorized_keys"

Y repetimos el comando con la IP del Nodo2:

    pi@Maestro:~ $ cat ~/.ssh/id_rsa.pub | ssh pi@192.168.1.15 "cat >> .ssh/authorized_keys"

A partir de este momento, cuando se haga un ssh desde el Maestro a los nodos esclavos con el usuario Pi, ya no será necesario poner la contraseña.

## Ejecución de los prrogramas en paralelo
Ya estamos en condiciones de ejecutar un programa de forma paralela en el cluster. Vamos a ejecutar 2.

### Helloworld.py
El primer programa que vamos a ejecutar es el clásico “Hola Mundo” que simplemente nos muestra un mensaje ejecutado desde cada uno de los nodos.

Para ello, desde el nodo maestro ejecutamos el siguiente comando:

    pi@Maestro:~ $ mpiexec -pernode -machinefile ~/machinefile python mpi4py/helloworld.py

El comando *mpiexec* es el que permite la ejecución del programa en modo paralelo.

El parámetro *–pernode* indica que queremos que se ejecute un proceso por Nodo (en nuestro caso 3 procesos, uno en el maestro y 2 en los esclavos).

Con el parámetro *–machinefile* especificamos la ruta del fichero *machinefile* que incluye las direcciones de todos los nodos del cluster.

Y por último lanzamos el intérprete de Python indicando la ruta donde se encuentra el programa a ejecutar (debe coincidir en todos los nodos).

El resultado debe ser un mensaje de saludo recibido desde cada nodo:

    Hello, World! I am process 0 of 3 on Maestro.
    Hello, World! I am process 2 of 3 on Nodo1.
    Hello, World! I am process 1 of 3 on Nodo2.

### Primos.py
El siguiente programa de ejemplo va a ser un sencillo programa de cálculo de números primos en el que le vamos a indicar un número y nos dirá cuántos números primos hay desde el 1 hasta el número que le hemos dado. Cuánto más alto sea el número que le demos más costoso se volverá el algoritmo y por tanto cuantos más procesadores involucremos en el cálculo mejor rendimiento tendremos.

Vamos a comenzar lanzando el proceso para que nos calcule los primos que hay entre el 1 y el 20000, pero utilizando un único nodo:

    pi@Maestro:~ $ mpiexec -np 1 -machinefile ~/machinefile python mpi4py/primos.py 20000

    El Proceso 0 de 1 en Maestro ha encontrado 2263 primos entre el 1 y el 20000
    Total de primos encontrados hasta el 20000: 2263
    Total time: 9.081415s

En este caso hemos sustituido el parámetro *–pernode* por *–np 1*,indicando que sólo deseamos que se ejecute un proceso.

Repetimos el comando pero utilizando los 3 nodos (esta vez con *–pernode*):

    pi@Maestro:~ $ mpiexec -pernode -machinefile ~/machinefile python mpi4py/primos.py 20000

    El Proceso 0 de 3 en Maestro ha encontrado 860 primos entre el 1 y el 6666
    El Proceso 1 de 3 en Nodo2 ha encontrado 724 primos entre el 6667 y el 13332
    El Proceso 2 de 3 en Nodo1 ha encontrado 679 primos entre el 13333 y el 20000
    Total de primos encontrados hasta el 20000: 2263
    Total time: 4.939719s

Como vemos, el tiempo empleado baja sustancialmente.

Puesto que la Raspberry Pi tiene un procesador ARM de 4 núcleos, y disponemos por tanto de un total de 12 núcleos, podemos ejecutar el comando anterior indicando que queremos 12 procesos simultáneos:

    pi@Maestro:~ $ mpiexec -np 12 -machinefile ~/machinefile python mpi4py/primos.py 20000
    El Proceso 0 de 12 en Maestro ha encontrado 262 primos entre el 1 y el 1666
    El Proceso 1 de 12 en Maestro ha encontrado 209 primos entre el 1667 y el 3332
    El Proceso 2 de 12 en Maestro ha encontrado 198 primos entre el 3333 y el 4998
    El Proceso 3 de 12 en Maestro ha encontrado 191 primos entre el 4999 y el 6664
    El Proceso 4 de 12 en Nodo2 ha encontrado 186 primos entre el 6665 y el 8330
    El Proceso 5 de 12 en Nodo2 ha encontrado 184 primos entre el 8331 y el 9996
    El Proceso 6 de 12 en Nodo2 ha encontrado 171 primos entre el 9997 y el 11662
    El Proceso 7 de 12 en Nodo2 ha encontrado 182 primos entre el 11663 y el 13328
    El Proceso 8 de 12 en Nodo1 ha encontrado 172 primos entre el 13329 y el 14994
    El Proceso 9 de 12 en Nodo1 ha encontrado 173 primos entre el 14995 y el 16660
    El Proceso 10 de 12 en Nodo1 ha encontrado 173 primos entre el 16661 y el 18326
    El Proceso 11 de 12 en Nodo1 ha encontrado 162 primos entre el 18327 y el 20000
    Total de primos encontrados hasta el 20000: 2263
    Total time: 2.487892s

El tiempo empleado vuelve a bajar, siendo mucho más eficiente el reparto de procesamiento.

Podemos ir cambiando el número que pasamos como parámetro así como el número de procesos e ir viendo cómo afecta al rendimiento.
