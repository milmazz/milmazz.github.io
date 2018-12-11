---
redirect_from: "/archivos/2005/05/19/reduciendo-el-tamano-de-tus-hojas-de-estilos-una-revision/"
author: milmazz
comments: true
date: 2005-05-19 21:50:57
layout: post
slug: reduciendo-el-tamano-de-tus-hojas-de-estilos-una-revision
title: Reduciendo el tamaño de tus hojas de estilos, una revisión
tags:
- CSS
- Recursos
---

Hace pocos días atrás comenté acerca de [la reducción en el tamaño en _bytes_ de
las hojas de estilo en
cascada](/article/2005/05/05/csscompiler-reduce-el-tamano-de-tus-hojas-de-estilos/)
a través del uso de
[CSScompiler](http://icebeat.bitacoras.com/archivo/121/csscompiler-10), en esta
ocasión presentaré otras herramientas que cumplen el mismo fin, unas lo llevan a
cabo mejor que otras.

[Nacho](http://www.microsiervos.com/archivo/general/nacho.html), uno de los
responsables de [Microsiervos](http://www.microsiervos.com/), nos presenta dos
alternativas más, la primera de ellas es [CSS
Optimiser](http://flumpcakes.co.uk/css/optimiser/), si lo desea puede leer
[acerca de CSS Optimiser](http://flumpcakes.co.uk/css/optimiser/about) para
obtener mayor información. De todas maneras, a continuación un resúmen de las
características de esta herramienta.

## Características de _CSS Optimiser_

* Elimina los comentarios.
* Elimina los espacios en blancos (por ejemplo, el exceso de espacios).
* Opción que permite convertir valores RGB a Hexadecimal (estos últimos son más
  pequeños).
* Convierte valores hexadecimales bajo el formato #RRGGBB a #RGB.
* Produce cambios en los valores, como por ejemplo `border: 1px 2px 1px 2px;` en
  `border: 1px 2px;`
* Convierte múltiples atributos de `background`, `font`, `margin`, `padding`,
  `list` en una simple lista de atributos.
* Convierte múltiples valores de la propiedad `border` en una simple lista de
  atributos.
* Se da la opción de convertir valores absolutos (por ejemplo: `px` o `pt`) en
  valores relativos (`em`).
* Agrupa atributos y valores de estilos que aparecen en varias ocasiones en un
  solo estilo.

Ahora bien, la segunda alternativa que nos plantea Nacho, [CSS
Compressor](http://cssc.darkriftstudios.com/) solo nos brinda la oportunidad de
eliminar los espacios en blancos y fusionar lo mayor posible el contenido, por
ejemplo:

Código fuente CSS original:

    body{
      /* Propiedades de fondo */
      background-color:#666666;
      background-image: url(image.png);
      background-position: bottom right;
      background-repeat: no-repeat;
      background-attachment: fixed;
      font-size: 100%;
    }

Resultado:

    body{background-color:#666666;background-image:url(image.png);background-position:bottom right;background-repeat:no-repeat;background-attachment:fixed;font-size:100%}

Para muchos este nuevo formato puede ser practicamente ilegible, es cierto, pero
en realidad ofrece cierta reducción al tamaño de las hojas de estilos en
cascada.

Continuando con el tema, [Zootropo](http://www.mundogeek.net/) nos propone en su
artículo [¡Adelgazar es
fácil!](http://mundogeek.net/archivos/2005/05/15/%c2%a1adelgazar-es-facil/) una
nueva herramienta a las expuestas anteriormente, esta herramienta es [CSS
Formatter and Optimiser](http://cdburnerxp.se/cssparse/css_optimiser.php), esta
herramienta llega a cumplir a cabalidad con la funcionalidad que propone CSS
Compressor, entre otras características, como la optimización del código CSS.
_CSS Formatter and Optimiser_ es una excelente herramienta, muy poderosa y posee
muchas opciones para el usuario. Vamos a describirlas brevemente.

## Opciones que ofrece _CSS Formatter and Optimiser_

* Compresión máxima (ninguna legibilidad, tamaño más pequeño)
* Compresión moderada (legibilidad moderada, tamaño reducido)
* Compresión estándar (equilibrio entre legibilidad y tamaño)
* Compresión baja (legilibilidad más alta)
* Compresión personalizada, puede elegir entre:
  * Ordenar los selectores.
  * Ordenar las propiedades.
  * Fusionar aquellos selectores que posean las mismas propiedades.
  * Fusionar aquellas propiedades en las que aplique el _shorthand CSS_.
  * Comprimir el formato del color, si el formato está en RGB se lleva a
    Hexadecimal, si está en hexadecimal también trata de reducir su formato en
    aquellos casos que aplique el _shorthand CSS_.
  * Convierte los selectores a minúsculas.
  * Casos especiales para las propiedades:
    * Convertir a minúsculas.
    * Convertir a mayúsculas.
    * Eliminar los simbolos `\` innecesarios.
  * Se ofrece la opción de guardar la salida en un fichero, lo que le permitirá
    ahorrar tiempo entre _copiar_ y _pegar_ en su editor de hojas de estilos en
    cascada favorito.

## Pruebas

A continuación se realizarán una serie de pruebas, estas estarán basadas
únicamente en dos parámetros, legibilidad y tamaño de los ficheros generados.
Todas las pruebas hechas parten de un mismo fichero CSS, él código mostrado en
este fichero presenta gran cantidad de comentarios y precario (adrede) uso de
_shorthands_.

Tabla de Resultados

	Herramienta
		Característica
		Antes
		Después
		Ahorro

CSS Optimiser

Caracteres

3143

601

2542

Lineas

128

41

N/A

Legibilidad

Muy alta

Alta

N/A

Porcentaje

N/A

N/A

81%

CSS Formatter & Optimiser

Caracteres

3143

802

2341

Lineas

128

57

N/A

Legibilidad

Muy alta

Alta

N/A

Porcentaje

N/A

N/A

74%

CSS Compressor

Caracteres

3143

1225

1918

Lineas

128

1

N/A

Legibilidad

Muy alta

Muy baja

N/A

Porcentaje

N/A

N/A

61%

CSScompiler

Caracteres

3143

1230

1913

Lineas

128

8

N/A

Legibilidad

Muy alta

Baja

N/A

Porcentaje

N/A

N/A

61%

N/A: No aplica

## Observaciones

_CSS Optimiser_ maneja muy bien la reducción de las declaraciones cuando es
aplicable el _shorthand_, algo en lo falla un poco _CSS Formatter & Optimiser_,
aunque éste último ofrece bastantes opciones, por lo que es bueno tomarlo en
cuenta a la hora de reducir el tamaño en _bytes_ de nuestras hojas de estilos.
Si queremos hacer uso de _CSS Optimiser_ y aún deseamos obtener una mayor
compresión, es posible obtenerla si combinamos el resultado obtenido con la
herramienta _CSS Compressor_, el cual eliminará los espacios existentes. Quizás
la única falla que percibi en _CSS Optimiser_ fue que aún no maneja
adecuadamente las reducciones de aquellas reglas que presentan declaraciones
comunes, leyendo ciertas notas del autor, me doy cuenta que está trabajando en
ello.

He trabajado con una hoja de estilos bastante comentada y sin utilizar
propiedades abreviadas (adrede) para realizar las pruebas, a continuación
muestro los enlaces a cada uno de los ficheros de las hojas de estilos.

* [Hoja sin ninguna compresión, original](http://blog.milmazz.com.ve/wp-content/pruebas-css/ejemplo.css)
* [Hoja de estilos utilizando CSS Optimiser](http://blog.milmazz.com.ve/wp-content/pruebas-css/css_optimiser.css)
* [Hoja de estilos utilizando CSS Fomatter & Optimiser](http://blog.milmazz.com.ve/wp-content/pruebas-css/css_formatter_and_optimiser.css)
* [Hoja de estilos utilizando CSS Compressor](http://blog.milmazz.com.ve/wp-content/pruebas-css/csscompressor.css)
* [Hoja de estilos utilizando CSScompiler](http://blog.milmazz.com.ve/wp-content/pruebas-css/csscompiler.css)

Si conoces alguna herramienta que permita la reducción del tamaño en _bytes_ de
las hojas de estilo en cascada no dudes en comentarlo, de esta manera, podría
ampliar la revisión nuevamente.
