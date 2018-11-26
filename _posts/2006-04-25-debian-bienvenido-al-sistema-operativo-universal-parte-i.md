---
redirect_from: "/archivos/2006/04/25/debian-bienvenido-al-sistema-operativo-universal-parte-i/"
author: milmazz
comments: true
date: 2006-04-25 15:04:46
layout: post
slug: debian-bienvenido-al-sistema-operativo-universal-parte-i
title: 'Debian: Bienvenido al Sistema Operativo Universal (Parte I)'
categories:
- debian
tags:
- debian
---

Desde el día ayer tengo en mi ordenador de escritorio **solo** [Debian](http://www.debian.org/), anteriormente tenía las tres versiones actuales de [Ubuntu](http://www.ubuntu.com/) (_warty_, _hoary_ y _breezy_), las cuales compartían la misma particion /home, sentía que estaba desperdiciando espacio, aunque siempre trataba de usar las tres versiones de Ubuntu, una buena excusa que encontre para arreglar las cosas fué la noticia en la que se anunciaba que el [período de soporte para Ubuntu 4.10 (_Warty Hedgehog_) estaba llegando a su fín](http://www.ubuntu.com/news/410eol), después del día 30 de Abril de 2006, no se incluirá información acerca de las actualizaciones de seguridad en los paquetes de Ubuntu 4.10.

Esta entrada viene a ser una recopilación de los pasos que he seguido para migrar de Ubuntu a Debian, a manera de recordatorio, espero pueda servirle a alguien más.

## ¿Por qué Debian?

Los siguientes puntos son solo opiniones personales.

  * El paso de Ubuntu a Debian es poco traumático.
  * Había trabajado con Debian en mi portátil por mucho tiempo.
  * Debian no depende de ninguna compañía ni del financiamiento de un solo hombre, es un trabajo realizado **solo** por una comunidad de voluntarios.
  * Existe **demasiada** (y **excelente**) documentación.
  * Tenía que probar algo nuevo, algo mejor, algunos amigos me convencieron, y aún más después del bautizo que me dio [José Parrella](http://bureado.com.ve/) en el canal IRC #velug en el servidor freenode.

## Primeros pasos

En primer lugar procedí a realizar respaldos de la información almacenada en programas como [Mozilla Thunderbird](http://www.mozilla.com/thunderbird/), [Liferea](http://liferea.sourceforge.net/), [Mozilla Firefox](http://www.mozilla.com/firefox/), bases de datos en [PostgreSQL](http://www.postgresql.org/), [MySQL](http://www.mysql.com/), entre otros. Este proceso es realmente sencillo.

En todos los programas mencionados anteriormente fué suficiente con copiar la raíz de su directorio correspondiente, por ejemplo:

  $ tar cvzf /path/respaldo/thunderbird.tgz ~/.thunderbird

El comando anterior generará un fichero empaquetado y comprimido en el directorio /path/respaldo/ (por supuesto, la ruta debe adaptarla según sus necesidades), dicho fichero contendrá la configuración personal e información almacenada por el programa _Mozilla Thunderbird_ (según el ejemplo) del usuario en cuestión.

## Obteniendo el CD de Debian

Puede [conseguir Debian](http://www.us.debian.org/distrib/) de muchas maneras, yo por lo menos descargue hace mucho tiempo la imagen del CD Debian GNU/Linux testing _Sarge_- Official Snapshot i386 Binary-1 por [Bitorrent](http://www.us.debian.org/CD/torrent-cd/), aunque también contaba con la imagen del CD de Debian 3.1r2, desgraciadamente no pude encontrarle.

## Analizando el esquema de particionamiento

Siempre acostumbro dedicarle un buen tiempo al esquema de particionamiento que utilizaré antes de proceder a instalar una distribución en particular, leyendo el [manual de instalación de Debian GNU/Linux 3.1](http://www.us.debian.org/releases/stable/installmanual) (le dije que existe una _excelente documentación_), específicamente en el **Apéndice B. Particionado en Debian**, tenía pensado hacer lo siguiente:

    Partición	Tamaño		Tipo
    /		200MB		ext2
    /usr		6GB		ext3
    /var		500MB		ext2
    /tmp		3GB		ext2
    /opt		200MB		ext2
    swap		1GB		Intercambio
    /home		Resto		ext3

Por supuesto, estaba siguiendo las recomendaciones del manual de instalación previamente mencionado, antes de continuar pregunte en el canal IRC #velug del servidor freenode y entre algunos amigos me recomendaron lo siguiente:

    Partición	Tamaño		Tipo
    /boot		80MB		ext2
    /tmp		3GB		ext2
    swap		1GB		Intercambio
    /		10GB		ext3
    /home		Resto		ext3

Algunos seguramente se preguntaran por qué se le ha asignado tanto espacio a la partición /tmp, simplemente porque continúamente estaré haciendo uso de herramientas para la creación de CDs ó DVDs, además de algunos programas multimedia y siempre he configurado mis [clientes Bittorrent](/article/2005/12/06/clientes-bittorrent/) para que hagan uso de esta partición antes de finalizar la descarga de los ficheros.

Por medidas de seguridad he establecido nodev, nosuid, noexec como opciones de montaje para la partición /tmp y las opciones ro, nodev, nosuid, noexec en la partición /boot.

## ¿Por qué a la final no he seguido las indicaciones del manual de instalación de Debian GNU/Linux?

Según un comentario que me hizo José Parrella, el cual más o menos decía así: cuando vas a utilizar el sistema Debian GNU/Linux para la casa, es muy probable que las particiones crezcan de manera desproporcionada y sin sentido (de acuerdo al uso que le dé el usuario), debido a esto es díficil adaptarlo a un modelo realmente conocido, entonces, la idea es no desperdiciar espacio alguno.

Resumiendo un poco la opinión de José:

  * No sabemos que vamos a hacer con la máquina.
  * Al no saberlo, no podemos particionarla más allá de lo que la lógica indica (separar /home, o /tmp).
  * Uno sabe que el sistema raíz de Debian, para un usuario en sus cabales (no soy uno de ellos), normalmente no excede los 4.5GB., muy probablemente menos.
  * A menos que sepamos **exactamente** la funcionalidad principal del Sistema Operativo, no podemos monitorear las particiones /var y /home.
  * La partición /tmp normalmente no debe exceder más de 1GB. de espacio, a menos que se haga uso intensivo de ella como prentendo hacerlo.
  * Si quisieramos un _servidor de archivos_, dejaríamos /var en la partición raíz y separaríamos /home.
  * Si quisieramos un _servidor de bases de datos_, dejaríamos /home en la partición raíz y separaríamos /var.

Sus razones me convencieron y desde mi punto de vista tenían lógica, sobretodo para alguien que le gusta hacer pruebas (a veces extremas) directamente desde su ordenador de escritorio.

En la siguiente entrega disertaré acerca del proceso de instalación del sistema base de Debian GNU/Linux y la puesta en marcha de la interfaz gráfica.

Correcciones, críticas constructivas siempre serán bien recibidas.

  *[GB.]: Gigabyte
  *[DVD]: Digital Versatile Disc
  *[IRC]: Internet Relay Chat
  *[CD]: Compact Disc
