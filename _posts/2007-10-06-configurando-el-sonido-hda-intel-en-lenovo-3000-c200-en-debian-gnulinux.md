---
redirect_from: "/archivos/2007/10/06/configurando-el-sonido-hda-intel-en-lenovo-3000-c200-en-debian-gnulinux/"
author: chantanito
comments: true
date: 2007-10-06 21:14:58
layout: post
slug: configurando-el-sonido-hda-intel-en-lenovo-3000-c200-en-debian-gnulinux
title: Configurando el sonido (HDA Intel) en Lenovo 3000 c200 en Debian GNU/Linux
categories:
- debian
- GNU/Linux
- Software
- Ubuntu
tags:
- alsa
- debian
- General
- GNU/Linux
- lenovo
- Software
- Ubuntu
---

La situación poco común se presentó con un portátil [Lenovo](http://www.lenovo.com), específicamente un 3000 c200; el computador en cuestión mostraba la tarjeta funcionando, como si estuviera todo normal, pero sucede que **no había sonido en lo absoluto** por más altos que estuvieran los indicadores gráficos del volumen. Indagando por [Google](http://www.google.com) me encontré que ya han habido muchos casos similares, no solamente para laptops Lenovo, sino para la mayoría que incluye ese tipo de tarjetas y me encontré con una solución en un [foro](http://help.ubuntu.com/community/HdaIntelSoundHowto) que me funcionó perfecto. Acá voy a tratar de explicar paso a paso todo lo que hice para que funcionara como debe ser.

Lo primero que se hizo fué asegurarse que se trata realmente de una tarjeta HDA Intel, con la siguiente línea de comandos:
    
    $ lspci | grep High

...a lo que se obtuvo la siguiente respuesta:

    00:1b.0 Audio device: Intel Corporation 82801G (ICH7 Family) High Definition Audio Controller (rev 02)

...donde se puede verificar que se trata de la HDA de la familia ICH7 de la [Intel](http://www.intel.com). Una vez verificado ésto, se procede a instalar algunos paquetes necesarios para que todo funcione de manera correcta, que son los siguientes:

  * build-essentials
  * gettext
  * libncurses5-dev

Ésto se logró con el aptitude, con la siguiente línea de comandos:

    $ sudo aptitude install el_paquete_que_quiero_instalar

Luego hay que descargar las cabeceras del kernel que se está usando. Para ésto, la manera más fácil de hacerlo fué instalando el paquete **module-assistant** y haciendo lo siguiente en una terminal:

    $ sudo m-a update
    $ sudo m-a prepare

Y el programa automáticamente va a saber cuáles cabeceras descargar y el directorio donde ponerlas. Cuando estén instalados éstos tres paquetes también se va a necesitar descargar de la [página del Proyecto Alsa](http://www.alsa-project.org) tres archivos necesarios y que son nombrados a continuación:

  * [alsa-driver-1.0.14.tar.bz2](ftp://ftp.alsa-project.org/pub/driver/alsa-driver-1.0.14.tar.bz2)
  * [alsa-lib-1.0.14a.tar.bz2](ftp://ftp.alsa-project.org/pub/lib/alsa-lib-1.0.14a.tar.bz2)
  * [alsa-utils-1.0.14.tar.bz2](ftp://ftp.alsa-project.org/pub/utils/alsa-utils-1.0.14.tar.bz2)

Se pueden descargar con un gestor de descargas preferido, ésto se hizo con **wget**, utilizando la línea de comandos:
    
    $ wget -c http://www.alsa-project.org/alsa-driver-1.0.14.tar.bz2

...y así para cada uno de los archivos. Cuando se tengan los tres archivos, se copian a la carpeta `/usr/src/alsa/` la cual, probablemente no existe todavía en el sistema y por lo tanto tendrá que ser creada; ésto se puede lograr con la siguiente línea de comandos:

    $ sudo mkdir /usr/src/alsa

...cuando se tenga el directorio, se copian los tres archivos tar.gz al mismo; ésto se puede lograr con:

    $ sudo cp alsa* /usr/src/alsa/

Luego hay que descomprimir los ficheros tar.gz con:

    $ sudo tar xvf el_archivo_que_vamos_a_descomprimir.tar.gz

Una vez descomprimidos nos ubicamos en la primera carpeta que va a ser alsa-driver-1.0.14/ y compilamos el alsa para las tarjetas HDA Intel con las siguientes líneas de comandos:

    $ sudo ./configure --with-cards=hda-intel
    $ sudo make
    $ sudo make install

Luego vamos a necesitar compilar los otros 2 paquetes restantes, para ello, nos ubicamos en la carpeta correspondiente y hacemos en una terminal lo siguiente:

    $ sudo ./configure
    $ sudo make
    $ sudo make install

Ésto se va a hacer tanto para _alsa-lib_ como para _alsa-utils_, pues el procedimiento es el mismo. Cuando se hayan compilado los tres paquetes el sistema ya debería ser capaz de reconocer correctamente la tarjeta y por lo tanto debe haber sonido; Ésto puedes ser verificado (1) Abriendo un reproductor de preferencia y reproduciendo algo de musica ó (2) Se puede hacer con la siguiente línea:

    $ cat /dev/urandom >> /dev/dsp/

Con lo cual se obtendrá un sonido algo parecido a unos aplausos, pero en realidad son sonidos producidos aleatoriamente.

Ésto debería ser todo. En las máquinas que se configuraron, cuando se conectaban los audífonos en el panel lateral, el sonido salía tanto por los audífonos como por las cornetas y al parecer se solucionó con una reiniciada, pero sino quieres reiniciar entonces lo que tienes que hacer es tumbar los módulos que se crearon y volverlos a cargar, tal cual reiniciaras el sistema:

    $ sudo modprobe -r snd_hda_intel652145
    $ sudo modprobe -r snd_pcm
    $ sudo modprobe -r snd_page_alloc

Luego para cargarlos hacemos las mismas línea, pero sin la opción -r.
