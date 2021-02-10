---
layout: page
title: Cluster
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

