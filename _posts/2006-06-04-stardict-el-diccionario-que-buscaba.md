---
redirect_from: "/archivos/2006/06/04/stardict-el-diccionario-que-buscaba/"
author: milmazz
comments: true
date: 2006-06-04 04:25:41
layout: post
slug: stardict-el-diccionario-que-buscaba
title: 'StarDict: El diccionario que buscaba'
categories:
- Software
tags:
- debian
- dictionary
- Software
- stardict
- Ubuntu
---

Leyendo el [ejemplar #14](http://www.tuxmagazine.com/node/1000198) de la revista [Tux Magazine](http://www.tuxmagazine.com/) me encuentro con un interesante artículo, _Learning Foreign Languages with jVLT and StarDict_, la segunda aplicación descrita en dicho artículo, [StartDict](http://stardict.sourceforge.net), llamó mi atención, así que a continuación se dará una breve revisión de la aplicación en cuestión.

## ¿Qué es StarDict?

_StarDict_ es un diccionario internacional multiplataforma escrito en Gtk2, puede ser utilizado sin conexión a la web.

## Características

 * _Búsqueda de patrones_: Usted puede buscar patrones de cadenas o caracteres usando comodines, por ejemplo, podrá usar el comodín `*` para buscar una cadena arbitraria, el resultado puede ser vacío, mientras que con el uso del comodín `?` buscará una coincidencia con un carácter arbitrario. e.g. Suponiendo que tiene instalado el diccionario _Inglés_ -- _Español_, al buscar el patrón hell? encontrará como resultado la traducción de hello, mientras que con el uso del patrón hell* encontrará todas aquellas posibles coincidencias que comiencen con la cadena hell, _hell_ (infierno en inglés), _hell_ (suerte en noruego), los resultados encontrados dependerá de los diccionarios que haya instalado.
 * _Búsqueda difusa_: Si usted por casualidad no recuerda exactamente como deletrear una palabra, podrá intentar realizar dicha búsqueda desde _StarDict_, éste utilizará el algoritmo de la _Distancia de Levenshtein_ El algoritmo de la _Distancia de Levenshtein_ o la _distancia de edición_ entre dos cadenas, se refiere al número mínimo de operaciones necesarias para transformar una cadena en otra, bajo éste algoritmo se considera una **operación** a la inserción, eliminación o substitución de un carácter. Para mayor información le recomiendo leer el artículo [Levenshtein Distance, in Three Flavors](http://www.merriampark.com/ld.htm).. Para utilizar esta característica simplemente comience la búsqueda con el carácter /, e.g., suponga que tiene instalado el diccionario _Español_ -- _Inglés_, usted cree que la palabra acero realmente se escribe así: asero, simplemente introduzca en el formulario la cadena /asero, obtendrá el resultado deseado.
 * _Búsqueda por palabras seleccionadas_: [![Scan Selection](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/scan-checkbox.png)](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/scan-checkbox.png)Si usted desea activar esta característica, deberá marcar la casilla de verificación que se encuentra en la parte inferior izquierda de la ventana principal de la aplicación. _StarDict_ automáticamente buscará palabras o frases que usted haya seleccionado en cualquier aplicación, esto incluye navegadores, [OpenOffice.org](http://www.openoffice.org/), etc., usted obtendrá un cuadro de dialogo que le mostrará la definición acerca de la palabra seleccionada.
 * _Manejo de diccionarios_: [![Manejo de diccionarios](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/manage-dictionaries-tooltip.png)](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/manage-dictionaries-tooltip.png)_StarDict_ le permite activar (desactivar) aquellos diccionarios que necesite (no necesite), también puede establecer el orden de búsqueda en los distintos diccionarios instalados. Todo lo anterior podrá realizarlo desde la sección [Manage Dictionaries](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/manage-dictionaries.png) (_Recurso:_ Imagen).
 * _¿No encuentra lo que necesita?_: _StarDict_ le permite realizar búsquedas en la web, solo deberá seleccionar el botón de búsqueda en Internet y escoger cualquiera de las 10 opciones actuales de búsqueda.

Ahora bien, seguramente esta aplicación resultará útil para muchas personas, si usted es uno de ellos y está interesado en instalarlo, es muy sencillo.

## ¿Cómo instalar StarDict?

Si usted es tan afortunado como yo, debe estar usando [Debian](http://www.debian.org/), así que simplemente tendrá que hacer:

    # aptitude install stardict

Ahora bien, si usted utiliza por ejemplo, [Ubuntu](http://www.ubuntu.com/), también puede instalarlo fácilmente, ¿cómo?, en primer lugar debe activar el repositorio _universe_ (recuerde actualizar la lista de paquetes disponibles), posteriormente debe hacer:

    $ sudo aptitude install stardict

Si usted utiliza otra distribución de GNU+Linux o es usuario de Windows, lea la documentación de la página oficial del proyecto, lamento informarle que este artículo es una revisión breve de la aplicación _StarDict_.

## ¿Cómo instalar diccionarios?

Primero que nada, debe descargar los diccionarios que necesite, para ello le recomiendo ir a la página de [Descarga de Diccionarios](http://stardict.sourceforge.net/Dictionaries.php) del proyecto.

Una vez que haya descargado los diccionarios, debe proceder como sigue:

    tar -C /usr/share/stardict/dic -x -v -j -f  \\
    diccionario.tar.bz2

## Personalizando la aplicación

[![Preferencias](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/preferences.png)](http://blog.milmazz.com.ve/wp-content/uploads/2006/06/preferences.png) Ya para finalizar, usted puede personalizar la aplicación desde la sección de _preferencias_, desde ella podrá modificar el comportamiento del programa.

Una característica de _StarDict_ que puede resultar realmente molesta es cuando se encuentra **activo** el modo _Scan_, es decir, toda palabra o frase resaltada con el ratón generará un cuadro de dialogo con la definición de dicha palabra o frase, si usted desea controlar este comportamiento, le recomiendo activar las siguientes casillas de verificación:

  * _Only do scanning while modifier key being pressed_.
  * _Hide floating window when modifier key pressed_.

Las casillas de verificación previamente mencionadas podrá encontrarlas bajo la sección _Dictionary_ -> _Scan Selection_.

Si usted esta inconforme con las opciones de búsqueda en Internet, puede agregar las que usted desea desde la sección _Main Window_ -> _Search Website_.

  *[Windows]: Te gusta autoflagelarte, ¿no?
  *[etc.]: etcétera
  *[e.g.]: exempli gratia
  *[GNU]: Gnu is Not Unix
