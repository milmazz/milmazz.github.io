---
redirect_from: "/archivos/2005/04/21/convirtiendo-los-formatos-de-compresion-de-audio/"
author: milmazz
comments: true
date: 2005-04-21 02:12:03
layout: post
slug: convirtiendo-los-formatos-de-compresion-de-audio
title: Convirtiendo los formatos de compresión de audio
tags:
- Scripts
---

Siguiendo con la temática que propuse en el artículo [Convirtiendo formatos de
audio OGG a MP3](/archivos/2005/04/16/convirtiendo-formatos-de-audio-ogg-a-mp3/)
he decido ampliar dicho _script_ para abarcar nuevos formatos. En esta ocasión
he decidido hacerlo un poco más interactivo con el usuario, aún faltan cosas,
pero las funciones elementales las cumple a cabalidad.

Las conversiones que se pueden realizar son las siguientes:

  * mp3 -> wav
  * mp3 -> ogg
  * ogg -> wav
  * ogg -> mp3
  * wav -> ogg
  * wav -> mp3

Antes de proseguir, vamos a revisar los requerimientos.

* `mpg321:` Reproductor **libre** de MP3 bajo linea de comandos, compatible con
  `mpg123`, este último no es libre. Nos permitirá decodificar ficheros MP3 (mp3
  -> wav).
* `vorbis-tools:` Paquete de herramientas de OGG Vorbis, entre las cuales se
  encuentran `oggenc`, para codificar ficheros WAV (wav -> ogg), y `oggdec`,
  para decodificar ficheros OGG Vorbis (ogg -> wav).
* `lame:` Codificar ficheros MP3 (wav -> mp3).
* `normalize:` Para ajustar o equilibrar el volúmen de los distintos ficheros
  involucrados, actúa sobre ficheros WAV.

Una breve descripción acerca de la funcionalidad del paquete `normalize` la
podrá encontrar en el artículo [Convirtiendo formatos de audio OGG a
MP3](/archivos/2005/04/16/convirtiendo-formatos-de-audio-ogg-a-mp3/).

Para aquellas personas que disfrutan de una distribución
[Debian](http://www.debian.org/) o alguna basada en ella simplemente deben hacer
lo siguiente como _superusuario_ o `root`.
    
    apt-get install lame mpg321 vorbis-tools normalize

El _script_ temporalmente actúa sobre los directorios actuales, aún no tiene la
capacidad de permitir como parámetros un directorio origen y un directorio
destino, próximamente implementaré esta opción, ante lo explicado anteriormente,
puede que le resulte conveniente copiar el _script_ dentro del directorio
`/usr/local/bin`, así podrá ejecutarlo desde cualquier directorio de su `$HOME`
sin inconveniente alguno.

Desde mi punto de vista el uso del _script_ es intuitivo, sin embargo, explicare
la evolución del proceso de conversión de 3 ficheros OGG Vorbis a MP3 con un
[ejemplo](/wp-content/ejemplo-de-uso-del-script-audioconverter.html). Puede
descargar el _script_ al hacer clic en
[AudioConverter](/wp-content/audioconverter-0.2.tgz).

### Cosas por hacer (TODO)

  * Permitir al usuario indicar un _directorio origen_ y un _directorio destino_
  * Permitir al usuario seleccionar si el formato de compresión de audio en un
    fichero mp3 sea CBR (Tasa Constante de Bits) o VBR (Tasa Variable de Bits)
  * Permitir al usuario seleccionar la calidad de codificación de los ficheros
    OGG Vorbis, de manera predeterminada está seleccionada como 3, en donde, -1
    representa la peor calidad y 10 la mejor.

### ¿Puedo colaborar?

Por supuesto, este _script_ es de uso **libre**, puede colaborar también al
comentar si existe algún fallo, o simplemente con sus sugerencias, las cuales
serán tomadas en cuenta para el posterior desarrollo del _script_.

### Anuncios

24/04/2005
Sale la versión [0.3](/wp-content/audioconverter-0.3.tgz) del _script_.

#### Características de la versión

Se añaden cuadros de dialogos con el uso de Zenity, permitiendo así interactuar
con el usuario de manera más amena.

22/04/2005
Sale la versión [0.2](/wp-content/audioconverter-0.2.tgz) del _script_.

#### Características de la versión

  * Se verifica que las aplicaciones necesarias se encuentren instaladas.
  * Se verifica la existencia de ficheros origen en el directorio donde es
    ejecutado el _script_.
  * Se incorpora la posibilidad de convertir ficheros WMA a MP3.
  * El _script_ renombra los ficheros para evitar posibles errores en las
    conversiones a realizar.
  * El nombre del ejecutable cambia a: audioconverter, de esta manera el comando
    es más homogéneo a los comandos regulares del _shell_.

### Agradecimientos

Muchas gracias Gabriel de [guia-ubuntu.org](http://www.guia-ubuntu.org/) por sus
comentarios y sugerencias.