---
redirect_from: "/archivos/2010/12/02/construyendo-de-manera-efectiva-y-rapida-imagenes-iso-de-debian-con-jigdo/"
author: milmazz
comments: true
date: 2010-12-02 14:04:39
layout: post
slug: construyendo-de-manera-efectiva-y-rapida-imagenes-iso-de-debian-con-jigdo
title: Construyendo de manera efectiva y rápida imágenes ISO de Debian con jigdo
categories:
- debian
tags:
- debian
- iso
- jigdo
---

Si usted desea el conjunto de CD o DVD para instalar Debian, tiene muchas
posibilidades, desde la [compra](http://www.debian.org/CD/vendors/) de los
mismos, muchos de los vendedores contribuyen con Debian. También puede realizar
descargas vía [HTTP/FTP](http://www.debian.org/CD/http-ftp/), vía
[torrent](http://www.debian.org/CD/torrent-cd/) o
[rsync](http://www.debian.org/CD/mirroring/rsync-mirrors). Pero en este artículo
se discutirá sobre un método para construir las imágenes ISO de Debian de manera
eficiente, sobretodo si cuenta con un repositorio local de paquetes, dicho
método se conoce de manera abreviada como [jigdo](http://atterer.org/jigdo/) o
_Jigsaw Download_.

Las ventajas que ofrece [jigdo](http://www.debian.org/CD/jigdo-cd/) están bien
claras en el portal de Debian, cito:

> ¿Por qué jigdo es mejor que una descarga directa?
>
>¡Porque es más rápido! Por varias razones, hay muchas menos réplicas para
>imágenes de CDs que para el archivo «normal» de Debian. Consecuentemente, si
>descarga desde una réplica de imágenes de CD, esa réplica no sólo estará más
>lejos de su ubicación, además estará sobrecargada, especialmente justo después
>de una publicación.
>
>Además, algunos tipos de imágenes no están disponibles para descarga completa
>como .iso porque no hay suficiente espacio en nuestros servidores para
>alojarlas.

Considero que la pregunta pertinente ahora es: _¿Cómo descargo la imagen con
jigdo?_.

En primer lugar, instalamos el paquete `jigdo-file`.

	# aptitude install jigdo-file

Mi objetivo era generar los 2 primeros CD para Debian Lenny, para la fecha de
publicación de este artículo la versión más reciente es la
[5.0.7](http://www.debian.org/News/2010/20101127). La lista de imágenes
oficiales para _jigdo_ las puede encontrar
[acá](http://www.debian.org/CD/jigdo-cd/#which).

	milmazz@manaslu /tmp $ cat files
	http://cdimage.debian.org/debian-cd/5.0.7/i386/jigdo-cd/debian-507-i386-CD-1.jigdo
	http://cdimage.debian.org/debian-cd/5.0.7/i386/jigdo-cd/debian-507-i386-CD-1.template
	http://cdimage.debian.org/debian-cd/5.0.7/i386/jigdo-cd/debian-507-i386-CD-2.jigdo
	http://cdimage.debian.org/debian-cd/5.0.7/i386/jigdo-cd/debian-507-i386-CD-2.template
	milmazz@manaslu /tmp $ wget -c -i files
	--2010-12-02 12:39:52--  http://cdimage.debian.org/debian-cd/5.0.7/i386/jigdo-cd/debian-507-i386-CD-1.jigdo
	Resolving cdimage.debian.org... 130.239.18.163, 130.239.18.173, 2001:6b0:e:2018::173, ...
	Connecting to cdimage.debian.org|130.239.18.163|:80... connected.
	HTTP request sent, awaiting response... 200 OK
	Length: 31737 (31K) [text/plain]
	Saving to: `debian-507-i386-CD-1.jigdo'

	100%[===================================================================================================================>] 31.737      44,7K/s   in 0,7s

	...

	FINISHED --2010-12-02 12:50:15--
	Downloaded: 4 files, 27M in 10m 21s (44,7 KB/s)
	milmazz@manaslu /tmp $ ls
	debian-507-i386-CD-1.jigdo  debian-507-i386-CD-1.template  debian-507-i386-CD-2.jigdo  debian-507-i386-CD-2.template files

Una vez descargados los ficheros necesarios, es hora de ejecutar el comando
`jigdo-lite`, siga las instrucciones del asistente.

	milmazz@manaslu ~ $ jigdo-lite debian-507-i386-CD-2.jigdo

	Jigsaw Download "lite"
	Copyright (C) 2001-2005  |  jigdo@
	Richard Atterer          |  atterer.net
	Loading settings from `/home/milmazz/.jigdo-lite'

	-----------------------------------------------------------------
	Images offered by `debian-507-i386-CD-2.jigdo':
	1: 'Debian GNU/Linux 5.0.7 "Lenny" - Official i386 CD Binary-2 20101127-16:55 (20101127)' (debian-507-i386-CD-2.iso)

	Further information about `debian-507-i386-CD-2.iso':
	Generated on Sat, 27 Nov 2010 17:02:14 +0000

	-----------------------------------------------------------------
	If you already have a previous version of the CD you are
	downloading, jigdo can re-use files on the old CD that are also
	present in the new image, and you do not need to download them
	again. Mount the old CD ROM and enter the path it is mounted under
	(e.g. `/mnt/cdrom').
	Alternatively, just press enter if you want to start downloading
	the remaining files.
	Files to scan:

El comando despliega información acerca de la imagen ISO que generará, en este
caso particular, `debian-507-i386-CD-2.iso`. Además, `jigdo-lite` puede
reutilizar ficheros que se encuentren en CD viejos y así no tener que
descargarlos de nuevo. Sin embargo, este no era mi caso así que presione la
tecla ENTER.

	-----------------------------------------------------------------
	The jigdo file refers to files stored on Debian mirrors. Please
	choose a Debian mirror as follows: Either enter a complete URL
	pointing to a mirror (in the form
	`ftp://ftp.debian.org/debian/'), or enter any regular expression
	for searching through the list of mirrors: Try a two-letter
	country code such as `de', or a country name like `United
	States', or a server name like `sunsite'.
	Debian mirror [http://debian.example.com/debian/]:

En esta fase `jigdo-lite` solicita la dirección **URL completa** de un
repositorio, aproveche la oportunidad de utilizar su repositorio local si es que
cuenta con uno. Luego de presionar la tecla ENTER es tiempo de relajarse y
esperar que `jigdo` descargue todos y cada uno de los ficheros que componen la
imagen ISO.

Luego de descargar los paquetes y realizar las operaciones necesarias para la
construcción de la imagen ISO `jigdo` le informará los resultados.

	FINISHED --2010-12-01 14:43:50--
	Downloaded: 6 files, 2,5M in 1,8s (1,39 MB/s)
	Found 6 of the 6 files required by the template
	Successfully created `debian-507-i386-CD-2.iso'

	-----------------------------------------------------------------
	Finished!
	The fact that you got this far is a strong indication that `debian-507-i386-CD-2.iso'
	was generated correctly. I will perform an additional, final check,
	which you can interrupt safely with Ctrl-C if you do not want to wait.

	OK: Checksums match, image is good!

Ahora bien, haciendo uso de un repositorio local, es bueno preguntarse en cuanto
tiempo aproximadamente puedes construir tu imagen ISO, en mi caso el tiempo de
construcción de `debian-507-i386-CD-2.iso` fue de:

	milmazz@manaslu ~ $ time jigdo-lite debian-507-i386-CD-2.jigdo

	...

	real	8m35.704s
	user	0m13.101s
	sys	0m16.569s

Nada mal, ¿no les parece?.

Ahora bien, haciendo uso de un repositorio local, es bueno preguntarse en cuanto
tiempo aproximadamente puedes construir tu imagen ISO, en mi caso el tiempo de
construcción de `debian-507-i386-CD-2.iso` fue de:

## Referencias

  * [jigdo - Jigsaw Download](http://atterer.org/jigdo)
  * [Descargar imágenes de CD de Debian con jigdo](http://www.debian.org/CD/jigdo-cd/)
  * [Debian Jigdo mini-HOWTO](http://atterer.org/jigdo/debian-jigdo-mini-howto)
