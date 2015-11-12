---
redirect_from: "/archivos/2005/12/06/clientes-bittorrent/"
author: milmazz
comments: true
date: 2005-12-06 17:41:36
layout: post
slug: clientes-bittorrent
title: Clientes BitTorrent
wordpress_id: 116
categories:
- Python
- Software
- Ubuntu
tags:
- azureus
- bittorrent
- freeloader
- Python
- rufus
- Software
- Ubuntu
---

Desde _mi punto de vista_ [Azureus](http://azureus.sourceforge.net/) es un cliente [BitTorrent](http://www.bittorrent.com/) que cae en los excesos, aparte de ello es demasiado lento y por si fuera poco consume una gran cantidad de recursos del sistema.

Si usted es usuario de [Ubuntu Linux](http://www.ubuntu.com), seguramente estará preguntándose, ¿por qué buscar un cliente BitTorrent si **Breezy** incluye uno? , bueno, si le soy sincero, ese cliente **apesta**, tiene muy pocas opciones.

En los siguientes párrafos veremos dos alternativas, que desde mi punto de vista tienen ciertas virtudes, las cuales muestro a continuación.


  * No caen en los excesos.
  * Son rápidos.
  * No consumen gran cantidad de recursos del sistema.
  * Ofrecen muchas opciones.

Sin mas preámbulos, les presento a [Rufus](http://rufus.sourceforge.net/) y [freeloader](http://www.ruinedsoft.com/freeloader/), clientes BitTorrents alternativos de gran envergadura.

## FreeLoader

Freeloader, es un manejador de descargas escrito en Python y brinda soporte a torrents.

Para instalar freeloader debemos seguir los siguientes pasos en Breezy.

    sudo aptitude install python-gnome2-extras python2.4-gamin

Seguidamente diríjase al sitio oficial de [freeloader](http://www.ruinedsoft.com/freeloader/) y descargue las fuentes del programa, para la fecha en la cual se redactó este artículo la versión más reciente de este programa es la **0.3**.

    wget http://www.ruinedsoft.com/freeloader/freeloader-0.3.tar.bz2

Luego de haber descargado el paquete proceda de la siguiente manera:

    $ tar xvjf freeloader-0.3.tar.bz2
    $ cd freeloader-0.3
    $ ./configure
    $ make
    $ sudo make install

Recuerde que para poder compilar paquetes desde las fuentes necesita tener instalado previamente el paquete `build-essential`

## Rufus

Rufus es otro cliente BitTorrent escrito en Python.

Vamos a aprovecharnos del hecho que existe una versión estable (0.6.9) compilada * para Breezy, los pasos son los siguientes:

    $ wget http://strikeforce.dyndns.org/files/breezy/rufus.0.6.9/rufus_0.6.9-0ubuntu1_i386.deb
    $ sudo dpkg -i rufus_0.6.9-0ubuntu1_i386.deb

* Esta versión ha sido compilada por [strikeforce](http://www.ubuntuforums.org/member.php?u=22492), para mayor información lea el hilo [Rufus .deb Package](http://www.ubuntuforums.org/showthread.php?t=57590&highlight=Rufus).
