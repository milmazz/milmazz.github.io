---
redirect_from: "/archivos/2005/05/04/imagemagick/"
author: milmazz
comments: true
date: 2005-05-04 07:00:44
layout: post
slug: imagemagick
title: ImageMagick
categories:
- GNU/Linux
- Software
tags:
- GNU/Linux
- Software
---

[ImageMagick](http://www.imagemagick.org/), es una _suite_ de software libre,
sirve para crear, editar y componer imágenes en mapa de bits. Puede leer,
convertir y crear imágenes en una larga variedad de formatos (más de 90 formatos
soportados). Las imágenes pueden ser cortadas, los colores pueden ser cambiados,
ciertos efectos pueden ser aplicados, las imágenes pueden rotadas y combinadas,
se pueden agregar a las imágenes, textos, líneas, polígonos, elipses, entre
otros.

Es una suite realmente potente bajo la línea de comandos, puede ver una serie de
[ejemplos](http://www.imagemagick.org/script/examples.php), esto le demostrará
lo que puede llegar a hacer con esta eficaz aplicación.

Esta herramienta me ha facilitado enormemente el procesamiento de gran cantidad
de imágenes que requieren el mismo tratamiento, solamente con una línea de
comandos voy a establecer todas las opciones que deseo. Por ejemplo:

    convert origen.jpg -resize 50% -bordercolor "#666"
    -border 4 destino.png

[Imagen Original](/images/2005-05-04-imagemagick/Tortugas_14.jpg) -> [Imagen
modificada con ImageMagick](/images/2005-05-04-imagemagick/Tortugas_14.png).

Con esta simple línea de comandos estoy realizando las siguientes conversiones
sobre la imagen origen.jpg, en primer lugar estoy reduciendo las proporciones de
anchura y altura al 50%, posteriormente estoy designando el color del borde al
valor hexadecimal (#666, gris), el ancho de dicho borde será de 4px, finalmente
estoy cambiando el formato de la imagen, estoy convirtiendo de jpg a png, los
resultados se podrán apreciar en la imagen destino.png. Ahora bien, imagínese
procesar 500 imágenes desde una aplicación con interfaz gráfica que le brinde la
rapidez que le ofrece ImageMagick, creo que hasta la fecha es imposible, por lo
que resulta un buen recurso el emplear esta herramienta para el procesamiento de
grandes lotes de imágenes.

Ahora vamos a "jugar" un poco para colocar una "marca de agua" a la imagen en
cuestión.

    convert origen.jpg -resize 50% -bordercolor "#666"
    -border 4 -font "Bitstream Vera Sans" -pointsize 16 -gravity SouthEast
    -fill "#FFF" -draw "text 10,10 'http://blog.milmazz.com.ve'" destino.png

[Imagen Original](/images/2005-05-04-imagemagick/Tortugas_18.jpg) -> [Imagen
modificada con ImageMagick](/images/2005-05-04-imagemagick/Tortugas_18.png).

Esta vez el número de opciones que he empleado han aumentado ligeramente. Vamos
a explicar brevemente cada una de ellas, la opción font nos permite elegir una
fuente, en el ejemplo he escogido la fuente Bitstream Vera Sans, la opción
pointsize permite escoger el tamaño de la fuente, la opción gravity nos permite
establecer la colocación horizontal y vertical del texto, he elegido la
primitiva SouthEast, la cual colocará el texto en la parte inferior derecha de
la imagen,  la opción fill me permite establecer el color de relleno del texto
que se dibujará con la opción draw, esta última opción escribirá la cadena
`http://blog.milmazz.com.ve`.

Para mayor información acerca de las distintas opciones que puede manejar le
recomiendo leer detenidamente la [descripción de las herramientas en línea de
comando de
ImageMagick](http://www.imagemagick.org/script/command-line-tools.php).
