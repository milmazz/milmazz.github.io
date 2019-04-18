---
title: Creando listas de reproducción para XMMS y MPlayer
author: milmazz
date: 2006-04-30 15:08:10
categories:
  - debian
  - GNU/Linux
  - Ubuntu
tags:
  - debian
  - find
  - GNU/Linux
  - mplayer
  - Scripts
  - Software
  - tips
  - trucos
  - Ubuntu
  - xmms
slug: creando-listas-de-reproduccion-para-xmms-y-mplayer
redirect_from: /archivos/2006/04/30/creando-listas-de-reproduccion-para-xmms-y-mplayer/
---

Normalmente acostumbro a respaldar toda la información que pueda en medios de
almacenamiento ópticos, sobretodo audio digital, ya sea en ficheros _[Ogg
Vorbis](http://www.vorbis.com/)_ o en _MPEG 1 Layer 3_. Desde hace poco más de
un año hasta la actualidad me he acostumbrado a mantener una estructura lógica,
la cual es más o menos como sigue:

/music/<nombre_artista>/<titulo_album>/<titulo_pista>

Pero hace mucho tiempo no era tan organizado en cuanto a la estructura de los
respaldos, entonces, la pregunta en cuestión es, ¿cómo lograr detectar la
presencia de ficheros de audio digital almacenados de manera persistente en un
dispositivo óptico de manera automática?

Al igual que lo expresado en la entrada [Eliminando ficheros inútiles de manera
recursiva](/article/2005/06/10/eliminando-ficheros-intiles-de-manera-recursiva/),
haremos uso del comando `find`.

Antes de entrar en detalle debo aclarar que voy a realizar una búsqueda
recursiva de ficheros en el _path_ correspondiente a mi unidad lectora de CDs.
Usted debe ajustar el _path_ por uno apropiado en su caso particular.

Si solo desea buscar ficheros _MPEG 1 Layer 3_:

    find /media/cdrom1/ -name \*.mp3 -fprint playlist

Pero si usted acostumbra a almacenar ficheros _Ogg Vorbis_ en conjunto con
ficheros _MPEG 1 Layer 3_, debería proceder así:

    find /media/cdrom1/ \( -name \*.mp3 -or -name \*.ogg \) -fprint playlist

El comando anterior también es aplicable para generar listas de reproducción de
video digital, en cuyo caso lo único que debe cambiar es la extensión de los
ficheros que desea buscar. El fichero que contendrá la lista de reproducción
generada en los casos expuestos previamente será playlist.

## Reproduciendo la lista generada

Para hacerlo desde [XMMS](http://www.xmms.org/) es realmente sencillo, acá una
muestra:

    xmms --play playlist --toggle-shuffle=on

Si usted **no** desea que las pistas en la lista de reproducción se reproduzcan
de manera **aleatoria**, cambie el argumento `on` de la opción
`--toggle-shuffle` por `off`, quedando como `--toggle-shuffle=off`.

Si desea hacerlo desde [MPlayer](http://www.mplayerhq.hu/) es aún más sencillo:

    mplayer --playlist playlist -shuffle

De nuevo, si **no** desea reproducir de manera aleatoria las pistas que se
encuentran en la lista de reproducción, elimine la opción del reproductor
MPlayer `-shuffle` del comando anterior.

Si usted desea suprimir la cantidad de información que le ofrece MPlayer al
reproducir una pista le recomiendo utilizar alguna de las opciones `-quiet` o
`-really-quiet`.
