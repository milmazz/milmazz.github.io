---
redirect_from: "/archivos/2005/07/24/creando-un-repositorio-local/"
author: milmazz
comments: true
date: 2005-07-24 17:31:55
layout: post
slug: creando-un-repositorio-local
title: Creando un repositorio local
categories:
- GNU/Linux
- Software
- Ubuntu
tags:
- GNU/Linux
- Software
- Ubuntu
---

## Planteamiento del Problema:

Mantener actualizada una laptop (u ordenador de escritorio) de bajos recursos,
con varios años de uso, el caso que se presenta es que el laptop en cuestión
posee solo un puerto para conexiones por modem de velocidades topes de 56kbps,
lo anterior puede ser traumático al realizar actualizaciones del sistema.

## Consideraciones:

Nos aprovecharemos del hecho de la disponibilidad de un puerto USB en el
ordenador de bajos recursos, el cual nos facilita en estos tiempos el
almacenamiento masivo de paquetes, en caso de no tener puerto
[USB](http://es.wikipedia.org/wiki/USB), podemos recurrir a unidades de CD como
medio de almacenamiento masivo.

Adicionalmente, aprovecharemos conexiones de gran bando de ancha, lo cual nos
facilitará la descarga de los paquetes necesarios desde los repositorios
disponibles en la red.

## Posible solución:

Después de plantear las consideraciones anteriores, una posible alternativa para
mantener actualizados nuestros ordenadores de bajos recursos es utilizar
dispositivos de almacenamiento masivo como repositorios locales, almacenando en
ellos solo los paquetes que sean realmente necesarios.

En los siguientes puntos trataré de ampliar la manera de crear un repositorio
local valiéndonos del uso de un [Pen
Drive](http://es.wikipedia.org/wiki/Pen_drive).

## Comenzamos:

En primer lugar vamos a respaldar los paquetes *.deb que se ubican en
/var/cache/apt/archives, esta actividad la vamos a realizar en el ordenador que
dispone de una conexión banda ancha.

    $ mkdir $HOME/backup
    $ sudo cp /var/cache/apt/archives/*.deb $HOME/backup

Después de respaldar los paquetes, vamos a remover todos los ficheros *.deb que
han sido descargados al directorio cache que almacena los paquetes, usualmente
esto quiere decir, eliminar todos los paquetes *.deb ubicados en
/var/cache/apt/archives, para ello hacemos lo siguiente:

    $ sudo aptitude clean

Ahora que se encuentra limpio el directorio cache procederemos a **descargar**
(sin instalar) los paquetes que sean necesarios para nuestro ordenador de bajos
recursos. Para ello hacemos lo siguiente:

    $ sudo aptitude -d install <paquetes>

El comando anterior simplemente descargará la lista de paquetes especificada
(recuerde que debe sustituir <paquetes> por la lista de paquetes que necesita),
la opción `-d` (equivalente a `--download-only`) nos permite realizar simples
descargas de paquetes, sin que estos se instalen, se ha usado el argumento
`install` para manejar las dependencias de dichos paquetes. En resumen, estamos
**descargando** (sin instalar) la lista de paquetes especificada y sus
dependencias.

Para trabajar mas cómodamente crearemos una carpeta temporal y desde allí
procederemos a crear nuestro repositorio local en cuestión, el cual finalmente
se guardará en el medio de almacenamiento masivo correspondiente.

    $ mkdir /tmp/debs
    $ mv /var/cache/apt/archives/*.deb /tmp/debs/
    $ cd /tmp

Estando ubicados en el directorio /tmp realizaremos una revisión de los paquetes
que se ubican en el directorio debs/ y crearemos el fichero comprimido
Packages.gz (el nombre del fichero debe ser exacto, no es cuestión de elección
personal).

    $ dpkg-scanpackages debs/.* | gzip > debs/Packages.gz

## Guardando el repositorio en un dispositivo de almacenamiento masivo

En nuestro caso procederé a explicar como almacenar el repositorio en un Pen
Drive, por ciertas cuestiones prácticas, primero, facilidad de movilidad que
presentan, la capacidad de almacenamiento, la cual regularmente es mayor a la de
un CD.

En la versión actual ([Hoary](http://www.ubuntulinux.org/504Released)) de
[Ubuntu Linux](http://www.ubuntulinux.org) los Pen Drive son montados
automáticamente, si esto no ocurre, puede realizar lo siguiente:

    $ sudo mount -t vfat /dev/sda1 /media/usbdisk

Recuerde ajustar los argumentos de acuerdo a sus posibilidades. Luego de montado
el dispositivo de almacenamiento masivo proceda a copiar de manera recursiva el
directorio debs/.

    $ cp -r /tmp/debs/ /media/usbdisk

## Estableciendo el medio de almacenamiento masivo como un repositorio.

### Si ha decidido utilizar un Pen Drive como medio de almacenamiento.

Después de copiar los ficheros en el medio de almacenamiento masivo, proceda a
desmontarlo (en nuestro caso: `$ sudo umount /media/usbdisk`)  y conectelo a su
ordenador de bajos recursos, recuerde que si el ordenador de bajos recursos no
monta automáticamente el dispositivo, debe montarlo manualmente como se explico
anteriormente.

Después de haber sido reconocido el dispositivo de almacenamiento masivo en el
ordenador de bajos recursos proceda a editar el fichero /etc/apt/sources.list y
agregue la linea deb file:/media/usbdisk debs/. Comente los demás repositorios
existentes, recuerde que para ello simplemente debe agregar el carácter
almohadilla (#) al principio de la linea que especifica a dicho repositorio.

### Si ha decidido utilizar un CD como medio de almacenamiento

Simplemente haciendo uso de `apt-cdrom add` le bastará, esto añadirá el medio a
la lista de recursos del fichero sources.list

## Finalizando...

Para finalizar deberá actualizar la lista de paquetes disponibles y proceder con
la instalación de dichos paquetes en el ordenador de bajos recursos, para ello,
simplemente bastará con hacer lo siguiente:

    $ sudo aptitude update
    $ sudo aptitude install <paquetes>

Recuerde restaurar en el ordenador que dispone de conexión banda ancha los
paquetes que se respaldaron previamente.

    $ sudo mv $HOME/backup/*.deb /var/cache/apt/archives/

¡Que lo disfrute! :D
