---
redirect_from: "/archivos/2006/11/22/creando-un-chroot-en-etch-amd64/"
author: chantanito
comments: true
date: 2006-11-22 19:54:07
layout: post
slug: creando-un-chroot-en-etch-amd64
title: Creando un chroot en Etch AMD64
categories:
- debian
- GNU/Linux
tags:
- debian
- GNU/Linux
---

Bien la cosa es que tratando de instalar el [Google Earth](http://earth.google.es/) en mi [Debian](http://www.debian.org) me he encontrado que no existe un paquete nativo para AMD64 (¿qué raro no?), por lo que me las he tenido que ingeniar para instalarlo. Nunca tuve la necesidad de hacer un chroot en el sistema ya que lo único que lo ameritaba era el Flash, pero no pensaba hacerme un chroot expresamente para el Flash y malgastar el espacio en mi disco, pero a la final siempre he tenido que hacerme uno!

Para aquellos que dominan el inglés se pueden leer el [Debian GNU/Linux AMD64 HOW-TO](https://alioth.debian.org/docman/view.php/30192/21/debian-amd64-howto.html) ya que los pasos para crear el chroot los he seguido de ahí. Claro que para aquellos que no tengan mucho tiempo o no sientan la necesidad de hacer un chroot siempre existe [ una manera rápida](https://alioth.debian.org/docman/view.php/30192/21/debian-amd64-howto.html#id292205) de hacer funcionar las cosas en un apuro.

Para aquellos que todavía no saben qué es un chroot les recomiendo que le echen un ojo a la [ Guía de referencia Debian](http://qref.sourceforge.net/Debian/reference/reference.es.html), de todas formas acá les cito textualmente de dicha guía lo que ellos definen como un chroot:

> ...el programa chroot, nos permite ejecutar diferentes instancias de un entorno GNU/Linux en un único sistema, simultáneamente y sin reiniciar...

El chroot en mi caso, está pensado para poder ejecutar aplicaciones a 32bits en un entorno 64 bits. El chroot es necesario ya que no se puede mezclar aplicaciones 32 bits con librerías 64 bits, por lo que se necesitan las librerías a 32 bits para correr dichas aplicaciones.

Lo que se hace en el chroot es instalar un sistema base para la arquitectura x86. Ésto lo puedes lograr haciendo en una terminal:

	# debootstrap --arch i386 sid /var/chroot/sid-ia32 http://ftp.debian.org/debian/

...para lo cual, posiblemente, vas a necesitar el paquete debootstrap. Ahora bien, ¿qué hace el `debootstrap`?; `debootstrap` es usado para crear un sistema base debian _from scratch_ (es algo como, desde la nada) sin tener que recurrir a la disponibilidad de `dpkg` ó `aptitude`. Todo ésto lo logra descargando los .deb de un servidor espejo y desempaquetándolos a un directorio, el cual, eventualmente sera utilizado con el comando ` chroot`. Por lo tanto la línea de comandos que introduciste anteriormente hace todo eso, sólo que lo vas a hacer para la rama _unstable_ ó sid, en el directorio /var/chroot/sid-ia32 y desde el servidor espejo específicado.

El proceso anterior puede demorar según sea la velocidad de tu conexión. No pude saber cuánto demoro el proceso en mi caso porque cuando empecé a hacerlo eran altas horas de la madrugada y me quedé dormido :(. Lo que tienes que saber es que cuando éste proceso finalice, ya tendrás un sistema base x86 o 32 bits en un disco duro en el directorio /var/chroot/sid-ia32. Una vez finalizado deberías instalar algunas librerías adicionales, pero para hacerlo deberás moverte al chroot y hacerlo con aptitude:

	# chroot /var/chroot/sid-ia32

...y luego instalas las librerías adicionales:

	# aptitude install libx11-6

Para poder ejecutar aplicaciones _dentro_ del chroot deberás tener también algunas partes del árbol de tu sistema 64 bits, lo cual puedes hacerlo mediante un **montaje enlazado**. El ejemplo a continuación, enlaza el directorio `/tmp` a el chroot para que éste pueda utilizar los "sockets" del X11, los cuales están en el `/tmp` de nuestro sistema 64 bits; y también enlaza el `/home` para que podamos accesarlo desde el chroot. También es aconsajable enlazar los directorios `/dev`, `/proc` y `/sys`. Para lograr ésto deberás editar tu `fstab` que se encuentra en `/etc` y añadir lo siguiente:

	# sid32 chroot
	/home   /var/chroot/sid-ia32/home none    bind      0       0
	/tmp     /var/chroot/sid-ia32/tmp  none     bind      0       0
	/dev     /var/chroot/sid-ia32/dev  none     bind      0       0
	/proc    /var/chroot/sid-ia32/proc none     bind      0       0

...y luego montarlas:

	# mount -a

Bien ya vamos llegando a final, unos cuántos pasos más y listo. Lo que necesitamos hacer a continuacióne es establecer los usuarios importantes en el chroot. La forma más rápida (sobretodo si tienes muchos usuarios) es copiar tus directorios `/etc/passwd`, `/etc/shadow` y `/etc/group` al chroot, a menos claro que quieras tomarte la molestia de añadirlos manualmente.

**ADVERTENCIA!** Cuando enlazas tu directorio `/home` al chroot, y borras éste último, todos tus datos personales se borrarán con éste, por consiguiente serán totalmente perdidos, por lo tanto debes recordar desmontar los enlaces antes de borrar el chroot.

## Corriendo aplicaciones en el `chroot`

Después de hacer todos los pasos anteriores, ya deberías poder ejecutar aplicaciones desde el chroot. Para poder ejecutar aplicaciones desde el chroot debes hacer en una terminal (en modo root):

	# chroot /var/chroot/sid-ia32

Luego deberás cambiarte al usuario con el que quieres ejecutar la aplicación:

	# su - usuario

Establecer $DISPLAY:

	# export DISPLAY=:0

Y finalmente ejecutar la aplicación que quieras, como por ejemplo, el firefox con el plugin de flash! Por supuesto deberás instalar la aplicación antes de ejecutarla, recuerda que lo que has instalado es un sistema base y algunas librerías adicionales.
