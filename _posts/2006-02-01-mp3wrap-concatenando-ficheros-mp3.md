---
redirect_from: "/archivos/2006/02/01/mp3wrap-concatenando-ficheros-mp3/"
author: milmazz
comments: true
date: 2006-02-01 02:15:23
layout: post
slug: mp3wrap-concatenando-ficheros-mp3
title: 'mp3wrap: Concatenando ficheros mp3'
categories:
- Software
- Ubuntu
tags:
- mp3
- mp3splt
- mp3wrap
- Software
- Ubuntu
---

[mp3wrap](http://mp3wrap.sourceforge.net/) es una utilidad en línea de comando que nos permite fusionar o concatenar dos o más ficheros `mp3`, todo esto sin perder los nombres de ficheros y la información de los [ID3](http://www.id3.org/), estándar que permite la inclusión de metadatos en contenedores multimedia. También es posible añadir otros ficheros que no sean `mp3`, como por ejemplo, listas de reproducción, ficheros de información, imágenes de portada. Claro, este proceso es posible revertirlo gracias a [mp3splt](http://mp3splt.sourceforge.net/), el cual describiré en un próximo artículo.

Con `mp3wrap`, usted puede fácilmente fusionar hasta un máximo de **255** ficheros en uno solo, lo cual pareciese ser suficiente para la mayoría. De igual manera, como se mencionó previamente, usted puede añadir ficheros que no sean `mp3`, pero hay algunas consideraciones al respecto.

  * Si el fichero es de texto, como pueden ser las listas de reproducción, los ficheros de información, entre otros, se **recomienda** que estos se ubiquen al principio del fichero a generar, puesto que el reproductor los descartará rápidamente.
  * Si el fichero es _binario_, como las imágenes por ejemplo, usted debe colocarlas al final del fichero a generar, de esta manera el reproductor se los encontrará después de reproducir y no los confundirá con ficheros `mp3`.

## Instalación del programa

Para poder instalar esta aplicación en Ubuntu, en primer lugar debemos tener activo en nuestro fichero `/etc/apt/sources.list` el repositorio `universe`, después de haber verificado esto procedemos a instalarlo.

    $ sudo aptitude install mp3wrap

A continuación explicaré el uso de `mp3wrap` a través de un ejemplo.

En primer lugar, mostraré la lista de ficheros a fusionar.

    $ ls
    01.mp3  02.mp3  03.mp3  04.mp3

Ahora fusionaré las 2 primeras canciones.
 
    $ mp3wrap album.mp3 01.mp3 02.mp3

He obviado el mensaje que nos muestra `mp3wrap` para evitar extender más de lo necesario este artículo. También es importante acotar que el fichero generado no se llamará `album.mp3` (lo que pareciese lógico), sino `album_MP3WRAP.mp3`, es recomendable **no** borrar la cadena `MP3WRAP`, ésta le indicará al programa `mp3splt`, el cual nos permite separar de nuevo los ficheros fusionados, que dicho fichero fué fusionado utilizando `mp3wrap`, lo anterior nos facilitará su extracción con `mp3splt`, en caso de darse.

Ahora bien, voy a añadir las otras dos canciones, que conste que este paso lo hago **solamente** para demostrar como añadir otros ficheros a una compilación previamente hecha con `mp3wrap`.

    $ mp3wrap -a album_MP3WRAP.mp3 03.mp3 04.mp3

Si deseamos conocer cuales son los archivos que contiene el fichero generado por `mp3wrap`, simplemente debemos hacer lo siguiente.

    mp3wrap -l album_MP3WRAP.mp3
    List of wrapped files in album_MP3WRAP.mp3:
    
    01.mp3
    02.mp3
    03.mp3
    04.mp3

Si en la instrucción anterior hubiesemos hecho uso de la opción `-v` (_verbose_), `mp3wrap` nos mostraría información adicional acerca de los ficheros. Por ejemplo:

    mp3wrap -lv album_MP3WRAP.mp3
    List of wrapped files in album_MP3WRAP.mp3:
      #    Size       Name
     --- --------   --------
      1) 6724962    01.mp3
      2) 9225205    02.mp3
     --- --------   --------
         15950240   2 files

Pueden observar en el ejemplo anterior que se nos muestra el tamaño en _bytes_ de cada uno de los ficheros, así como el número total de ficheros que han sido fusionados y su tamaño correspondiente en _bytes_.

Como se ha podido ver a través del articulo el uso de `mp3wrap` es bastante sencillo, si tiene alguna duda acerca de su uso consulte el manual de `mp3wrap`, `man mp3wrap`, o la sección de [preguntas mas frecuentes acerca de mp3wrap](http://mp3wrap.sourceforge.net/faq.html).
