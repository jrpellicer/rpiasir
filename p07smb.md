---
layout: page
title: NAS con SMB
---

# DISCO NAS CON SMB

## SAMBA
Windows utiliza un protocolo de archivos compartidos llamado CIFS que es el que permite compartir archivos, carpetas, impresoras y demás recursos en una red de equipos Windows. Este protocolo antiguamente se llamaba SMB, y es de este nombre de donde sale el nombre de Samba. Samba es una implementación libre del protocolo CIFS para sistemas Unix/Linux de manera que es posible que ordenadores con Linux, Mac OS X o Unix se vean como servidores o actúen como clientes en redes de Windows.

En esta práctica vamos a hacer que nuestra Raspberry pueda compartir datos del disco duro dentro de una red Windows. Vamos a utilizar el disco duro extraíble.

## Preparación del disco duro
Los datos que vamos a compartir serán datos de nuestro disco duro extraíble. Ya sabemos cómo conectar y trabajar con un disco duro extraíble con Raspbian.

En primer lugar vamos a conectar el disco duro extraíble a nuestra máquina Windows. En la raíz del disco duro vamos a crear una carpeta que se llame *Compartida* y en ella pondremos algún documento o foto.

Esta carpeta será (como su nombre indica) la que compartiremos al resto de los equipos desde la Raspberry.

Extraemos el disco duro del equipo Windows y lo conectamos a la Raspberry como ya sabemos.

Raspbian lo reconocerá automáticamente. Ejecutamos *lsblk*:

    $ lsblk

    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda           8:0    0 931,5G  0 disk
    └─sda1        8:1    0 931,5G  0 part
    mmcblk0     179:0    0  14,5G  0 disk
    ├─mmcblk0p1 179:1    0   256M  0 part /boot
    └─mmcblk0p2 179:2    0  14,2G  0 part /

Observamos que el disco */dev/sda* no está montado de manera automática. Tendremos que montarlo.

Vamos a montarlo. En primer lugar creamos un directorio en */media* llamado *disco_ext* donde montaremos el disco:

    $ sudo mkdir /media/disco_ext

Procedemos al montaje indicando la partición que queremos montar y el directorio donde será montado:

    $ sudo mount /dev/sda1 /media/disco_ext

Y ya está listo para ser utilizado. Habrá que tener ojo con los permisos, puesto que los mandatos los hemos hecho con el sudo, y por consiguiente el propietario del directorio será root.

**ATENCIÓN**. Si apagamos la Raspberry o la reiniciamos, habrá que volver a montar el disco, a no ser que escribamos en */etc/fstab*.

Para que se monte cada vez de manera automática tecleamos:

    $ sudo bash –c “echo ‘/dev/sda1 /media/disco_ext ext4 defaults 0 0’ >> /etc/fstab”

## Instalación de SAMBA
En primer lugar instalamos Samba con apt-get:

    $ sudo apt-get update     
    $ sudo apt-get install samba

Comprobamos si el servicio está ejecutándose una vez se ha instalado:

    $ sudo service smbd status
Debe aparecer como que el servicio está running (ejecutándose).

Y comprobamos también la versión instalada:

    $ smbd --version

## Configuración de SAMBA
Una vez hechas las comprobaciones de que el SMB está instalado y corriendo, vamos a configurarlo. Para ello vamos a modificar el archivo *smb.conf*:

    $ sudo nano /etc/samba/smb.conf

Comprobamos que en la sección \[global\] aparece el nombre del grupo de trabajo que tenemos en nuestra red Windows (por defecto es WORKGROUP):

    workgroup = WORKGROUP

Nos vamos al final del fichero y añadimos una nueva sección:

    [Compartida]
    path = /media/disco_ext/Compartida
    browseable = yes
    guest ok = no
    writable = yes
    valid users = @samba

Estamos indicando que la carpeta a compartir es la que en su momento llamamos Compartida en la raíz del disco duro. Ello lo hacemos indicando la ruta de la carpeta en el parámetro path.

En los demás parámetros estamos indicando los permisos de acceso y forzando a que los únicos usuarios que pueden acceder son los que están incluidos en el grupo samba.

Una vez cambiado el archivo de configuración indicando el recurso a compartir, vamos a crear los usuarios y grupos necesarios para acceder desde Windows.

En primer lugar creamos el grupo samba que es el que hemos indicado en el archivo de configuración que tiene acceso al recurso compartido:

    $ sudo groupadd samba

A continuación creamos un usuario con el que conectarnos. Podríamos llamarlo igual que el usuario de Windows desde donde queremos acceder (en este ejemplo pongo smbuser):

    $ sudo adduser smbuser

Nos habrá pedido un password durante el proceso de creación del usuario. Además hay que separar esta contraseña de la propia de acceso de samba. En nuestro caso ponemos la misma. Ejecutamos:

    $ sudo smbpasswd -a smbuser

Añadimos el usuario creado al grupo samba creado anteriormente.

    $ sudo gpasswd -a smbuser samba

El grupo samba necesita tener permisos de lectura, escritura y ejecución en la carpeta compartida (*/media/disco_ext/Compartida*). Se los damos ejecutando el comando setfacl. El modelo de permisos que se ha empleado desde la creación de Linux se denomina DAC (Control de Acceso Discrecional). En el sector de la seguridad se considera insuficiente. Existen otros modelos como ACL (comandos setfacl o getfacl), MAC y RBAC (utilidad SELinux). En este caso utilizamos el modelo ACL. Como estos comandos no vienen instalados por defecto, previamente ejecutamos:

    $ sudo apt-get install acl

Y luego ya asignamos los permisos:

    $ sudo setfacl -R -m "g:samba:rwx" /media/ disco_ext/Compartida

Sólo nos queda reiniciar el servicio para que los cambios tengan efecto:

    $ sudo systemctl restart smbd

## Acceso desde WINDOWS
Una vez instalado y configurado, podemos ir a un equipo Windows, y en el explorador de archivos acceder a la Red. Desde ahí podemos entrar en la Raspberry, y mediante los credenciales que configuramos en Samba, acceder a los recursos compartidos.

Es posible que de primeras no nos aparezca la Raspberry en la Red de Windows. Podemos forzar la conexión a una Unidad de Red. Para ello abrimos una consola de comandos de Windows y tecleamos lo siguiente (sustutuyendo la IP por la de la Raspberry y el password por el que le pusimos):

    > net use \\192.168.1.136\Compartida /delete
    > net use \\192.168.1.136\Compartida /user:smbuser password

Es interesante asignarle una unidad de Red (por ejemplo la Z:) de manera que cada vez que arranque el ordenador Windows aparezca la conexión a la carpeta compartida de la Raspberry.

