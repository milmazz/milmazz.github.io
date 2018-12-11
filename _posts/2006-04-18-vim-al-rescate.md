---
redirect_from: "/archivos/2006/04/18/vim-al-rescate/"
author: milmazz
comments: true
date: 2006-04-18 17:40:36
layout: post
slug: vim-al-rescate
title: Vim al rescate
categories:
- GNU/Linux
- Software
tags:
- GNU/Linux
- Software
- vim
---

Al examinar el día de hoy el último fichero de respaldo de la base de datos de este _blog_, me percate que existe una cantidad **inmensa** de registros que en realidad no me hacen falta, sobretodo respecto a las estadísticas, es increible que los registros de una simple base de datos llegara a ocupar unos 24MB, dicha información no tiene mayor relevancia para los lectores puesto que dichos datos suelen ser visualizados en la interfaz administrativa del _blog_, pero al ocupar mayor espacio en la base de datos, pueden retardar las consultas de los usuarios. Por lo tanto, era necesario realizar una limpieza y eliminar unos cuantos _plugins_ que generaban los registros de las estadísticas.

Ahora bien, imagínese abrir un documento de **266.257** líneas, **24.601.803** carácteres desde algun editor de textos gráfico, eso sería un **crimen**. ¿Qué podemos hacer?, la única respuesta razonable es utilizar [Vim](http://www.vim.org).

[Vim](http://www.vim.org) es un **avanzado editor** de textos que intenta proporcionar todas las funcionalidades del editor _de facto_ en los sistemas _*nix_, **Vi**. De manera adicional, proporciona muchas otras características interesantes. Mientras que **Vi** funciona solo bajo ambientes *nix, _Vim_ es compatible con sistemas Macintosh, Amiga, OS/2, MS-Windows, VMS, QNX y otros sistemas, en donde por supuesto se encuentran los sistemas *nix.

A continuación detallo más o menos lo que hice:

En primer lugar respalde la base de datos del _blog_, enseguida procedí a descomprimir el fichero y revisarlo desde _Vim_.

    $ vim wordpress.sql

Comence a buscar todos los `CREATE TABLE` que me interesaban. Para realizar esto, simplemente desde el modo **normal** de _Vim_ escribí lo siguiente:

    /CREATE TABLE <strong><Enter></strong>

Por supuesto, el <Enter> anterior se refiere a presionar la tecla, **no lo escriba**. Para volver a realizar la búsqueda hacia adelante (hacia atrás) solamente presionaba la tecla n (Shift + n), a la final me di cuenta que todas las tablas que me interesaban estaban de manera secuencial y separadas de aquellas tablas que no me interesaban, entonces recorde que desde el mismo _Vim_ es posible _guardar en otro fichero partes del fichero actual_, antes de continuar debía conocer el principio y el final de la líneas a copiar, simplemente me posicione al principio de la línea en cuestión y en modo **normal** teclee Ctrl + g, el cual me permite conocer el número de línea actual y da información acerca del número de líneas del fichero, como mi copia debía llegar hasta el final del fichero ya tenía toda la información que necesitaba.

Con todo la información necesaria, lo único que restaba por hacer era copiar la sección que me interesaba en otro fichero, para ello debemos proceder como sigue desde el modo normal de _Vim_:

    :264843,266257 w milmazz.sql

El comando anterior es muy sencillo de interpretar: Copia todo el contenido encontrado desde la línea 264.843 hasta la línea 266.257 y guardalo en el fichero milmazz.sql.

Inmediatamente restaure el contenido de mi base de datos y **listo**.

Algunos datos interesantes.

## Fichero original

 * Tamaño: 24MB
 * Número total de líneas: 266.257

## Fichero resultado

 * Tamaño: 1.2MB
 * Número total de líneas: 1.415

**Tiempo aproximado de trabajo:** 4 minutos.

¿Crees que tu editor favorito puede hacer todo esto y más en menos tiempo?. Te invito a que hagas la prueba ;)

**That's All Folks!**
