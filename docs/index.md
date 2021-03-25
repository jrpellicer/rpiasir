---
layout: page
title: Inicio
excerpt: Proyectos con Raspberry Pi en 1º ASIR
nav_order: 1
---
# La Raspberry Pi en el aula de 1º de ASIR
Esta es una recopilación de pequeños proyectos para realizar en el aula de 1º del CFGS de ASIR con Raspberry Pi en los que se ven contenidos de manera transversal, o si se prefiere al revés, son proyectos en los que se ven contenidos de 1º de ASIR mientras se ve Linux en una Raspberry Pi de manera transversal. El objetivo final es aprender contenidos mientras se aprende Linux jugando con una Raspberry Pi.

## A quién va dirigido este documento
Como profesor técnico de FP de ASIR me encuentro todos los años con alumnos a los que les cuesta mucho incorporar el sistema operativo Linux a su vida cotidiana. La mayoría de los alumnos que entran en primero no han trabajado nunca con este sistema, y los que lo han hecho son alumnos que han cursado previamente el ciclo medio de SMR y que, a su vez, muestran ciertos reparos (en general) a trabajar con Linux.

Durante años estoy impartiendo la asignatura de primer curso de Implantación de Sistemas Operativos, en la que después de pasar por Windows 10 y Windows Server, estudiamos Linux partiendo desde cero y profundizando todo lo que el tiempo y el denso temario nos permite. Soy consciente todos los años de que, a pesar de las prácticas que hacemos, no son suficientes para que los alumnos se desenvuelvan con naturalidad en Linux durante el segundo curso. Es necesario que en otros módulos de primero se refuerce el trabajo con Linux de manera transversal para que los alumnos hagan una inmersión.

Puesto que conocía la Raspberry Pi y el potencial que ofrece, y me parecía un “juguete” ideal para aprender jugando, decidí sacar una lista de posibles proyectos atractivos para los alumnos con la Raspberry Pi para desarrollar en clase, pero no en el módulo de Implantación de Sistemas Operativos, sino en un módulo que, en mi opinión, se presta más a desarrollar otros contenidos mientras se ve Linux de manera transversal como es el de Fundamentos del Hardware. Así que, durante el segundo y tercer trimestre de este curso pasado, me lancé con ese módulo a trabajar en clase con la Raspberry Pi; a que conocieran DAS, SAN y NAS de primera mano, a que crearan una nube privada o vieran la virtualización a nivel de sistema operativo en un miniordenador de bajo costo como si de un potente servidor se tratara.

Este documento va dirigido a aquellos profesores que deseen incorporar la Raspberry Pi en sus proyectos de clase, especialmente a los de primer curso del ciclo de ASIR en la que los alumnos se desenvolverán desde el minuto 1 con un sistema Linux en la que tendrán que tocar la configuración de la red, acceder a su router doméstico, escanear IPs, conectarse por SSH con certificados de clave pública, ejecutar un programa Python, administrar usuarios, permisos, contenedores, etc. En resumen, trabajar con un servidor real de unos 40€.
Por supuesto está dirigido a todo aquel maker que desee hacer pequeños proyectos DIY de informática doméstica y conocer todas las posibilidades que ofrece la Raspberry Pi y su ecosistema, e incluso combinándola con Arduino y otras placas con microcontroladores.

## Marco de los proyectos
Como he dicho, los proyectos los he diseñado para trabajar en el aula (y/o en casa) enmarcados dentro del módulo de Fundamentos del Hardware de primero de ASIR. Considero que muchos de ellos se ajustan, como complemento a los apuntes teóricos, a los contenidos que el currículum enumera en este módulo. Es cierto que también hay muchos de los contenidos del módulo de Implantación de Sistemas Operativos que se relacionan directamente con los proyectos que aquí se enumeran, pero es el módulo de Fundamentos del Hardware el idóneo para desarrollarlos por la amplitud de sus contenidos.

También es cierto que en algunos de los proyectos se tocan contenidos de módulos de segundo, pero del modo que se ven no considero que haya colisión ni que sea mayor problema el haberlos visto, sino al contrario, una ventaja que estén familiarizados.

## Objetivos de los proyectos
Para cada uno de los proyectos que se detallan en este documento se han establecido los siguientes objetivos:
- **Puesta en marcha de la Raspberry Pi:** El objetivo de este proyecto es conocer la arquitectura del equipo, decidir la versión del sistema operativo, e instalarlo mediante el flasheado de la tarjeta. Una vez instalado habrá que administrar usuarios y grupos locales, configurar el protocolo TCP/IP y acceder al sistema por SSH para administrarlo remotamente.
- **Los puertos GPIO:** Con este proyecto conoceremos el hardware de la Raspberry Pi y los elementos de conexión y ampliación del sistema mediante los puertos GPIO.
- **Docker:** Aprenderemos cómo funciona la virtualización a nivel de sistema operativo y a trabajar con herramientas de virtualización con contenedores.
- **Owncloud:** En este proyecto el objetivo es conocer el Cloud computing y el almacenamiento en la nube (StaaS). Crearemos una nube privada de almacenamiento y aprenderemos a instalar y probar aplicaciones mediante repositorios y mediante contenedores.
- **Disco DAS:** Aquí vamos a conocer los sistemas de almacenamiento y los modos de conexión de discos. Conectaremos un disco duro externo por un puerto USB, trabajaremos con sistemas de archivos mediante comandos y veremos cómo administrar discos, particiones y volúmenes. 
- **Disco NAS con OMV:** En este caso la conexión del disco será también mediante USB pero para proporcionar una conexión NAS a otros equipos. Para ello veremos cómo instalar una aplicación, trabajaremos con sistemas de archivos mediante herramientas gráficas (un entorno Web), crearemos un servidor de archivos con un sistema NAS, administrando el acceso a recursos con SAMBA/CIFS y accederemos a los recursos compartidos desde una máquina Windows.
- **Disco NAS con SMB:** Al igual que en el anterior proyecto crearemos un disco NAS pero en este caso utilizaremos el protocolo SMB/CIFS mediante comandos. Veremos cómo gestionar permisos de red y permisos locales y modificaremos archivos de configuración del sistema. 
- **Disco SAN:** El objetivo de este proyecto es conocer el modo de conexión de discos SAN en contraposición de los NAS y DAS de los proyectos anteriores. Crearemos un disco SAN y lo conectaremos a una máquina Windows por conexión iSCSI. 
- **RAID de discos:** El objetivo es conectar 2 discos duros extraíbles y montar un sistema de discos RAID-0 y medir los tiempos de lectura/escritura.
- **OCS Inventory:** Es necesario tener inventariado el hardware de los equipos que componen todo el sistema informático de una organización. Veremos cómo hacerlo mediante la instalación de  herramientas para el inventariado hardware automático o desatendido.
- **Nagios:** Del mismo modo es necesario tener correctamente configurados y verificados los equipos mediante herramientas de chequeo y diagnóstico y monitorización. Nagios es una potente herramienta que podemos instalar en una Raspberry para centralizar la monitorización de nuestros servidores y equipos.
- **Cluster de Docker Swarm:** Si en el proyecto de Docker vimos cómo trabajar con herramientas de virtualización, en este proyecto conoceremos las arquitecturas de alta disponibilidad, un orquestador de contenedores y cómo se montan estos ordenadores en bastidores o racks.
- **Cluster de computación paralela:** Los mismos objetivos que en el proyacto anterior, pero esta vez aplicados a la computación en paralelo.

Para finalizar he incluido un proyecto integrador, de IoT con Arduino y Raspberry Pi, cuyos objetivos se escapan de los propuestos para alumnos de primer curso de ASIR, pero que sirve de ampliación para todos aquellos que deseen profundizar en los proyectos que se presentan en este documento o iniciarse en el IoT junto con Arduino. Consiste en la realización de una pequeña estación meteorológica en la que se hacen lecturas de temperatura y humedad y se almacenan los datos en una pequeña base de datos para después ser accedidos mediante una página web.

## Material necesario
Para desarrollar los proyectos se necesita que cada alumno cuente con una Raspberry Pi, junto con su tarjeta SD y su conector de alimentación. Opcionalmente se puede adquirir una carcasa para alojar la Raspberry Pi (con o sin refrigeración) y además un lector de tarjeta SD (o un adaptador similar) para poder conectarla al ordenador si éste no dispone de lector de tarjetas.

En algunos de los proyectos es necesario que se conecte un disco duro externo mediante un conector USB. Prácticamente todos los alumnos disponen de un disco duro externo para uso particular que podrían utilizar para la realización de estos proyectos.

Existen kits en el mercado de Raspberry Pi + Tarjeta + Conector + Cable HDMI + Carcasa por unos 80-90 € En nuestro caso el cable HDMI no va a ser necesario para la realización de ningún proyecto, pues nos conectaremos por terminal vía SSH a nuestro sistema.

Se proponen 2 proyectos para realizar en grupo consistentes en la creación de clusters de RPis, en los cuales se pueden utilizar las Raspberrys de cada alumno. Además,el proyecto del RAID también se puede realizar en grupos de 3 alumnos, utilizando 2 de las tarjetas SD como discos duros.
