---
redirect_from: "/archivos/2007/02/07/identificar-el-hardware-de-tu-pc-en-debian-gnulinux/"
author: chantanito
comments: true
date: 2007-02-07 18:16:45
layout: post
slug: identificar-el-hardware-de-tu-pc-en-debian-gnulinux
title: Identificar el Hardware de tu PC en Debian GNU/Linux
categories:
- debian
- GNU/Linux
- Software
tags:
- debian
- General
- GNU/Linux
- Recursos
- Software
---

Bien, lo que vamos a hacer a continuación es muy fácil, tán fácil como instalar un paquete, luego ejecutarlo y leer la información que él nos "escupe" (me encanta como suena :) ). Como sabrán, soy usuario de [Debian GNU/Linux](http://www.debian.org) en su versión Etch para la arquitectura AMD64, pero ésto en realidad no es tan relevante ya que el paquete se encuentra tanto en Testing como en Stable para la mayoría de las arquitecturas y en la sección main de los repositorios.

Lo que vamos a instalar es el paquete **lshw-gtk**, que bien como dice en la descripción del paquete: _"es una pequeña herramienta que provee información detallada de la configuración de hardware de la máquina. Puede reportar la configuración exacta de la memoria, versión de firmware, configuración de la tarjeta madre, versión del procesador y su velocidad, configuración de la caché, velocidad del bus, etc. en sistemas x86 con soporte DMI, en algunas máquinas PowerPC (se sabe de su funcionamiento en las PowerMac G4) y ADM64"_.

Como ya sabrán, para instalar el paquete es tan sencillo como abrir una terminal y escribir en modo superusuario los siguiente:

     # aptitude install lshw-gtk

El paquete no es muy pesado, de hecho, con todo y dependencias a penas ha de superar el mega de información, por lo que el proceso de instalación es rápido (si se tiene una conexión decente claro).

Una vez instalado el paquete no tenemos que hacer más que ejecutarlo. Para poder ejecutarlo debemos hacerlo desde una terminal, ya que según tengo entendido, no se instala en los menús del Gnome. Así que debemos escribir en una terminal (en modo superusuario):

     # lshw-gtk

y listo, se ejecutará perfectamente, dejándonos navegar por unos paneles donde se encuentran los distintos componentes de nuestro sistema.

Si lo que quieres es tener un lanzador en los menús del Gnome, es muy sencillo, sólo deberás crear uno de la siguiente manera. Abre una terminal en modo superusuario y escribe lo siguiente:

    gedit /usr/share/applications/LSHW.desktop

luego de presionar la tecla Enter se abrirá una ventana con el gedit en la cual deberás pegar el siguiente texto:

    [Desktop Entry]
    Name=LSHW
    Comment=Identifica el hardware del sistema
    Exec=gksu lshw-gtk
    Icon=(el icono que les guste)
    Terminal=false
    Type=Application
    Categories=Application;System;

Y ya con eso deberías tener tu lanzador en el menú Aplicaciones --> Herramientas del sistema. Vas a necesitar de permisos de superusuario para poder ejecutarlo.

Acá una imagen de como se ve el lshw-gtk:

![lshw-gtk](http://farm1.static.flickr.com/136/383086183_d88bb36d78.jpg?v=0)
