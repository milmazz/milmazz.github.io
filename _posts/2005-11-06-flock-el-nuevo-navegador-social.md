---
redirect_from: "/archivos/2005/11/06/flock-el-nuevo-navegador-social/"
author: milmazz
comments: true
date: 2005-11-06 14:58:36
layout: post
slug: flock-el-nuevo-navegador-social
title: Flock, el nuevo navegador social
wordpress_id: 98
categories:
- Software
- Ubuntu
tags:
- Software
- Ubuntu
---

[Flock](http://www.flock.com/), es un nuevo navegador que toma sus bases en [Mozilla](http://www.mozilla.org/) [Firefox](http://www.mozilla.org/products/firefox/), su objetivo es captar la atención de usuarios que suelen usar herramientas de comunicación social que están en _boga_, como por ejemplo:

 * [del.icio.us](http://del.icio.us/): Almacena y comparte tus enlaces favoritos.
 * [Flickr](http://flickr.com/): Almacena y comparte tus imágenes.
 * [Technorati](http://technorati.com/): Entérate acerca de lo que se habla actualmente en los _blogs_. Colección de enlaces a bitácoras organizados por etiquetas o _tags_.
 * Sistemas de Blogging: Entre ellos: [WordPress](http://wordpress.org/), [Blogger](http://www.blogger.com/), [Movable Type](http://www.sixapart.com/movabletype/), entre otros.

## Sistema de publicación

Respecto a la posibilidad de publicar entradas o _posts_ en tu _blog_ desde el mismo navegador, Flock le ofrece una ventana aparte, tendrá que rellenar apropiadamente las distintas opciones que se le muestran para configurar el programa y de esa manera comenzar a redactar sus noticias, artículos, entre otros.

[![Flick topbar en Flock](http://photos25.flickr.com/60473225_19138118af_m.jpg)](http://flickr.com/photos/66721368@N00/60473225) Siguiendo con el tema de la publicación de artículos, Flock, le permite conectarse con su cuenta en Flickr y añadir fotos, esta posibilidad no se restringe solo a las cuentas de Flickr, podrá incluir fotos que se muestren en otros sitios, solamente deberá arrastrar dicha imagen a la interfaz que le proporciona el editor en cuestión.

De igual manera lo explicado en el párrafo anterior puede aplicarse al texto, podrá arrastrar a la zona de edición cualquier texto disponible en la web, tambien Flock ofrece una opción denominada blog this, su funcionamiento es muy sencillo, solamente deberá seleccionar un texto que le interese publicar, seguidamente proceda a dar click con el botón derecho del _mouse_ blog this, el texto en cuestión aparecerá en la zona de edición como una cita.

El sistema de publicación que le ofrece Flock le permite guardar sus artículos como _borradores_ o marcarlos para su publicación inmediata, otra característica que cabe resaltar es la posibilidad de indicar explícitamente con cuales etiquetas o _tags_ desea que se almacene la _entrada_ para su clasificación en Technorati.

## Favoritos

[![URLs y manejo de favoritos en Flock](http://photos29.flickr.com/60473226_cdbf8a45ba_m.jpg)](http://flickr.com/photos/66721368@N00/60473226) El sistema de favoritos se integra con tu cuenta en del.icio.us, gestor de enlaces favoritos o _bookmarks_ comunitario. y organizado por etiquetas o _tags_.

## Lectura de feeds

[![Lectura de Feeds en Flock](http://photos33.flickr.com/60473224_aa3b667b6a_m.jpg)](http://flickr.com/photos/66721368@N00/60473224) Flock nos indica cuando un _blog_ o bitácora dispone de un _feed_, la manera de indicarlo es muy agradable a la vista, simplemente muestra un icono al lado derecho de la ventana de la URL. Si lo desea, puede ver el contenido del _feed_ desde el mismo Flock, que le ofrece un visualizador de _feeds_, en él podrá ordenar las entradas por fechas o por la fuente, de manera adicional podrá contraer o expander todas las noticias, si decide contraer (o expander) las noticias de acuerdo al orden que haya elegido (por fecha o por fuente), puede ir expandiendo (o contrayendo) dichas noticias una por una.

## ¿Desea probar Flock?

Si lo desea, puede probar fácilmente Flock al hacer uso de los ficheros binarios que se ofrecen, en [ubuntu](http://www.ubuntulinux.org) (aplicable en otras distribuciones) debe hacerse lo siguiente:

En primer lugar deberá descargar el paquete binario que se ofrece para la plataforma Linux desde la sección [Developer](http://www.flock.com/developer/) de Flock.

Antes de continuar, debe saber que Flock está compilado haciendo uso de `libstdc++` en su versión 5, si, se encuentra en [Breezy](http://www.ubuntulinux.org/newsitems/release510), debe instalarla de la siguiente manera:

    $ sudo aptitude install libstdc++5

Una vez que se haya completado la transferencia del paquete binario de Flock, debe ubicarse en el directorio destino de la descarga y proceder a descompimir y desempaquetar el paquete en cuestion, para ello, debe hacer lo siguiente.

    $ tar xvzf flock-0.4.10.en-US.linux-i686.tar.gz

Por supuesto, es de suponerse que en este ejemplo particular el paquete que se descargó fué `flock-0.4.10.en-US.linux-i686.tar.gz`, usted debe ajustar este argumento de acuerdo al fichero que haya descargado.

Una vez culminado el paso anterior lo que sigue es sencillo, debe entrar en el directorio generado y ejecutar el comando `flock`, más o menos similar a lo que sigue a continuacion.

    $ cd flock
    $ ./flock

Recuerde que en Breezy usted puede generar una entrada al menú haciendo uso de _Herramientas del Sistema_ -> _Applications Menu Editor_, seguidamente seleccione el submenu _Internet_ y genere una nueva entrada con las siguientes propiedades.

 * Name: Flock
 * Comment: Internet Browser
 * Command: /_directorio_donde_esta_flock_/flock
 * Icon: /_directorio_donde_esta_flock_/icons/mozicon128.png

## Referencias:

  * [Cómo pasar extensiones de Firefox a Flock](http://www.blogpocket.com/2005/10/24/como-pasar-extensiones-de-firefox-a-flock/).
  * [Probando Flock, el navegador social](http://www.dobleclic.org/probando-flock-el-navegador-social/)
  * [13 cosas que puedes hacer con Flock (y como hacerlas) en Español](http://nuhuati.zoomblog.com/archivo/2005/10/21/13-cosas-que-puedes-hacer-con-Flock-y-.html).
