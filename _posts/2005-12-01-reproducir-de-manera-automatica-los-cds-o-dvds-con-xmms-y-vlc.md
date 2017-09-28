---
redirect_from: "/archivos/2005/12/01/reproducir-de-manera-automatica-los-cds-o-dvds-con-xmms-y-vlc/"
author: milmazz
comments: true
date: 2005-12-01 02:51:21
layout: post
slug: reproducir-de-manera-automatica-los-cds-o-dvds-con-xmms-y-vlc
title: Reproducir de manera automática los CDs o DVDs con XMMS y VLC
categories:
- Ubuntu
tags:
- Ubuntu
- vlc
- xmms
---

Si desea reproducir automáticamente los CDs de audio (o DVDs) al ser insertados con [XMMS](http://www.xmms.org/) (o con [VLC](www.videolan.org/vlc/)) simplemente cumpla los siguientes pasos:

En primer lugar diríjase a _Sistema -> Preferencias -> Unidades y soportes extraíbles_, desde la lengüeta **Multimedia** proceda de la siguiente manera:

## Si desea reproducir automáticamente un CD de sonido al insertarlo:

  1. Marque la casilla de verificación _Reproducir CD de sonido al insertarlo_
  2. En la sección de comando escriba lo siguiente: `xmms -p /media/cdrom0`

## Si desea reproducir automáticamente un DVD de vídeo al insertarlo

  1. Marque la casilla de verificación _Reproducir DVD de vídeo al insertarlo_
  2. En la sección de comando escriba lo siguiente: `wxvlc dvd:///dev/dvd`

**Nota:** En XMMS puede ser necesario configurar el _plugin_ de entrada de audio que se refiere al _Reproductor de CD de audio_ (`libcdaudio.so`), puede configurarlo desde las preferencias del programa.
