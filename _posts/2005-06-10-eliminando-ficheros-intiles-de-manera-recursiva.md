---
title: Eliminando ficheros inútiles de manera recursiva
author: milmazz
date: 2005-06-10 13:37:35
tags:
  - Scripts
slug: eliminando-ficheros-intiles-de-manera-recursiva
redirect_from: /archivos/2005/06/10/eliminando-ficheros-intiles-de-manera-recursiva/
---

En algunos casos mientras redactamos, codificamos o trabajamos en algunos editores de texto se van generando ficheros temporales que puede irse acumulando en nuestros directorios, estos suelen ser utiles en aquellos casos en los cuales las aplicaciones terminan de manera inesperada, seguramente podremos recuperar los ultimos cambios hechos al utilizar este tipo de ficheros, o en el caso de los ficheros `core`, nos pueden servir en aquellos casos en los cuales alguna funcion de nuestros programas no funciona como deberia y genera una _violacion de segmento_, los ficheros `core` nos pueden facilitar el analisis en la busqueda de los posibles errores en la funcion.

En muchas ocasiones nos encontramos que estos ficheros temporales se encuentran dispersos en algunos directorios y el hecho de borrarlos **uno a uno** suele ser un proceso mas bien tedioso. Por la razon mencionada anteriormente podriamos hacernos la siguiente pregunta, ¿es posible automatizar el proceso de eliminacion de ficheros "inutiles" de manera _recursiva_?, la respuesta es **si**.

El siguiente _script_ nos ayudara servira para lograr lo que deseamos.

    #!/bin/bash

    #Borrar de manera recursiva los ficheros inutiles.

    echo Directorio Raiz: $PWD
    echo Procesando...

    find $PWD \( -name \*~ -or -name \*.o -or -name \*\# -or -name core \) -exec rm -vf {} \;

    echo Listo!

En el codigo mostrado anteriormente el comando que realiza **todo** el trabajo por nosotros es `find`, voy a explicar brevemente que hace este comando.

El comando `find` necesita de un _camino_ o _ruta_ y de una _expresion regular_ para lograr encontrar alguna coincidencia al recorrer el arbol de directorios cuya **raiz** es el _camino_ especificado, `find` evaluara de izquierda a derecha las expresiones indicadas, tomando en cuenta las reglas de precedencia en los operadores, al conocer el resultado (_cierto_ o _falso_) `find` continuara con el siguiente fichero.

Dentro del comando `find` encontrara el uso de ciertas opciones, entre las cuales cabe mencionar las siguientes:

* `-or`: Representa el **o logico**, es equivalente a utilizar la opcion `-o`.
* `-exec`: Ejecuta la orden especificada siempre y cuando `find` haya encontrado alguna concordancia con la expresion regular, por lo tanto se devuelve valor cierto. Las ordenes seran aquellos argumentos que siguen a `-exec` hasta que encontrar el caracter **;** (punto y coma). Si desea ser consultado antes de realizar la ejecucion al encontrarse alguna coincidencia, es preferible hacer uso de la opcion `-ok`.
* `-name`: Especifica la base del nombre del fichero que deseamos buscar, no es necesario especificar el directorio, hace distincion entre mayusculas y minusculas. Se puede hacer uso de metacaracteres.
* `{}`: Cadena que es reemplazada por el nombre del fichero que se esta procesando en ese instante.

Puede copiar el _script_ mostrado arriba, supongamos que lo ha llamado `rmnull`, **debe** moverlo dentro del directorio `/usr/local/bin/` (haciendolo como superusuario). Posteriormente debe otorgarle permisos de ejecucion.

    $ sudo mv rmnull /usr/local/bin/
    chmod +x /usr/local/bin/rmnull

Ahora bien, para hacer uso del _script_ simplemente debera teclear en consola `rmnull`, el _directorio raiz_ sera el directorio en el que se encuentre actualmente. Veamos un ejemplo de ejecucion del _script_.

    milton@omega:~$ touch file# file.o file~ pruebas/file# pruebas/file~ pruebas/core
    milton@omega:~$ pwd
    /home/milton
    milton@omega:~$ rmnull
    Directorio Raiz: /home/milton
    Procesando...
    «/home/milton/Desktop/find.txt~» borrado
    «/home/milton/pruebas/file#» borrado
    «/home/milton/pruebas/file~» borrado
    «/home/milton/pruebas/core» borrado
    «/home/milton/file#» borrado
    «/home/milton/file.o» borrado
    «/home/milton/file~» borrado
    Listo!

En el ejemplo de ejecucion hago uso del comando `touch` para crear los ficheros especificados (en caso de no existir), estos archivos en principio se encuentran vacios y con permisos de lectura y escritura para el dueño del fichero, grupo al pertenece el dueño y demas usuarios. Posteriormente hago uso del comando `pwd` para conocer mi ubicacion actual, a continuacion "invoco" al _script_ `rmnull` quien hara el trabajo de limpieza de manera automatizada.
