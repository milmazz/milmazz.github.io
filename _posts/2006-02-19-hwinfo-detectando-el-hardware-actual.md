---
redirect_from: "/archivos/2006/02/19/hwinfo-detectando-el-hardware-actual/"
author: milmazz
comments: true
date: 2006-02-19 02:20:49
layout: post
slug: hwinfo-detectando-el-hardware-actual
title: 'hwinfo: Detectando el hardware actual'
categories:
- GNU/Linux
- Software
- Ubuntu
tags:
- GNU/Linux
- hardware
- hwinfo
- linux
- Software
- Ubuntu
---

`hwinfo` es un programa que nos permite conocer rápidamente el _hardware_ detectado actualmente en nuestros ordenadores, por ejemplo, si deseamos obtener los datos de dispositivo SCSI, simplemente utilizamos el comando `hwinfo --scsi`.

Para instalar este programa en _Ubuntu Linux_ en primer lugar debemos tener activados el repositorio `universe`, seguidamente haremos uso de `aptitude`, tal cual como sigue:

    $ sudo aptitude install hwinfo

Si deseamos conocer el uso de este programa de manera detallada, simplemente escribimos `hwinfo --help`.

En el caso que haga uso del comando `hwinfo` **sin** parámetro alguno nos mostrará la _lista completa_ del hardware detectado actualmente, es importante resaltar que esta lista puede ser muy extensa, por lo cual le recomiendo hacer uso de un _pipe_ para administrar la salida generada por `hwinfo` y poder visualizarla página a página, tal cual como sigue.

    $ hwinfo | less

Si por el contrario, usted solo desea conocer una lista resumida del hardware detectado haga uso del parámetro `--short`, lo anterior quedaría de la siguiente manera:

    $ hwinfo --short

Este programa nos brinda bastantes opciones, es recomendable hacer uso de los parámetros cuando necesitamos información referente a un dispositivo en específico, por ejemplo, si deseamos conocer la información acerca de la tarjeta de sonido, hacemos lo siguiente:

    $ hwinfo --sound

Como le mencione anteriormente, para conocer en detalle las opciones que nos brinda este programa, le recomiendo leer el manual. Espero sea de provecho ;)
