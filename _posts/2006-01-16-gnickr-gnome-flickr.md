---
redirect_from: "/archivos/2006/01/16/gnickr-gnome-flickr/"
author: milmazz
comments: true
date: 2006-01-16 14:51:05
layout: post
slug: gnickr-gnome-flickr
title: 'Gnickr: Gnome + Flickr '
categories:
- Software
- Ubuntu
tags:
- flickr
- gnickr
- gnome
- linux
- Software
- Ubuntu
---

[Gnickr](http://gnickr.sourceforge.net/) le permite manejar las fotos de su cuenta del sitio [Flickr](http://flickr.com/) como si fueran archivos locales de su escritorio [Gnome](http://gnome.org/). Todo lo anterior lo hace creando un sistema de ficheros virtual de su cuenta en Flickr.

Hasta ahora, Gnickr le permite realizar las siguientes operaciones:

  * Subir fotos.
  * Renombrar fotos y _set_ de fotos.
  * Borrar fotos.
  * Insertar fotos en _sets_ previamente creados.
  * Eficiente subida de fotos, escala las imágenes a `1024 x 768`

Se planea que en futuras versiones se pueda editar la descripción de cada foto, la creación/eliminación de _sets_ de fotos, establecer las opciones de privacidad en cada una de las fotos, así como también integrar el proceso de autorización en _nautilus_.

Si desea instalar Gnickr, previamente debe cumplir con los siguientes requisitos.

  * Gnome 2.12
  * Python 2.4
  * gnome-python >= 2.12.3
  * Librería de imágenes de Python (PIL)

## Instalando Gnickr en Ubuntu Breezy

En primer lugar debemos actualizar el paquete `gnome-python` (en ubuntu recibe el nombre de `python2.4-gnome2`) como mínimo a la versión `2.12.3`, para ello descargamos el paquete [python2.4-gnome2_2.12.1-0ubuntu2_i386.deb](http://prdownloads.sourceforge.net/gnickr/python2.4-gnome2_2.12.1-0ubuntu2_i386.deb?download).

Seguidamente descargamos el paquete [Gnickr-0.0.3](http://prdownloads.sourceforge.net/gnickr/gnickr_0.0.3-1_i386.deb?download) para Ubuntu Breezy. Una vez descargados los paquetes procedemos a instalar cada uno de ellos, para ello hacemos.

    $ sudo dpkg -i python2.4-gnome2_2.12.1-0ubuntu2_i386.deb
    $ sudo dpkg -i gnickr_0.0.3-1_i386.deb

Una vez que hemos instalado el paquete Gnickr para Ubuntu Breezy debemos autorizarlo en nuestra cuenta Flickr para que éste programa pueda manipular las fotos, para ello hacemos lo siguiente.
    
    $ gnickr-auth.py

Simplemente debe seguiremos las instrucciones que nos indica el cuadro de dialogo. Una vez completado el proceso de autorización **debe** reiniciar `nautilus`.

    $ pkill nautilus

## Uso de Gnickr

El manejo de Gnickr es muy sencillo, para acceder a sus fotos en su cuenta Flickr simplemente apunte nautilus a `flickr:///`.

    $ nautilus flickr:///

También puede ver las fotos de cualquier otra cuenta en Flickr apuntando a `flickr://[nombreusuario]`.

Para agregar fotos a un _set_, simplemente arrastre desde la carpeta **Unsorted** hasta la carpeta que representa el _set_ de fotos que usted desea, lo anterior también puede aplicarse para mover una foto de un _set_ a otro.

Para renombrar una foto, simplemente modifique el nombre del fichero de la foto.
