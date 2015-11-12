---
redirect_from: "/archivos/2005/04/16/convirtiendo-formatos-de-audio-ogg-a-mp3/"
author: milmazz
comments: true
date: 2005-04-16 18:54:31
layout: post
slug: convirtiendo-formatos-de-audio-ogg-a-mp3
title: Convirtiendo formatos de audio OGG a MP3
wordpress_id: 5
tags:
- Scripts
---

Por todos es bien sabido la superioridad que presenta el formato de  compresión de audio [OGG Vorbis](http://www.vorbis.com/) frente al MP3, adicionalmente, el primero de los formatos es **libre**, no posee patentes, algo que, en el caso del MP3 no es cierto, [el MP3 **sí** posee licencia](http://www.mp3licensing.com/royalty/software.html). Desgraciadamente no siempre lo mejor es lo más difundido, solo espero que esta situación cambie algún día.

Solo en algunas ocasiones es "preferible" hacer uso de los MP3, por ejemplo, mi reproductor portátil únicamente acepta los formatos Mp3 y WMA, dos formatos privativos, el primero de ellos fué desarrollado por [Fraunhofer ISS](http://www.iis.fraunhofer.de/) y el segundo por [Microsoft](http://www.microsoft.com/).

Ahora bien, después de una breve introducción, vamos a lo nuestro, el _script_ en primera instancia removerá los espacios en los nombres de los ficheros OGG Vorbis, seguidamente convertirá todos los carácteres en mayúsculas a minúsculas, esto lo hacemos con [`rename`](/archivos/2005/04/14/renombrando-ficheros-con-rename), a continuación decodificaremos el fichero con `oggdec`, este último creará ficheros WAV. Luego de haber creado los ficheros WAV, vamos a ajustar el volumen de dichos ficheros a un nivel _standard_, con esto evitamos la posible existencia de una variación muy drástica en el volumen de una canción a otra, sobretodo si se tienen colecciones de álbumes distintos, con niveles de grabación diferentes, este ajuste lo realizamos con el comando `normalize`.

Ya para finalizar, el último bucle del _script_ convertirá los ficheros WAV en MP3, de manera predeterminada se codificará el formato de compresión de audio a unos 160kbps, ud. puede modificar este comportamiento pasándole al _script_ el único argumento que éste acepta, por ejemplo:
    
    ogg2mp3 192

Esto codificará el fichero MP3 a 192kbps. Ya para finalizar, si no hay errores en la codificación de los ficheros, se procederá a borrar los ficheros WAV, que han sido usados como temporales. 

    #!/bin/bash
    
    #Removiendo Espacios
    rename 'y/\ /_/' *.ogg
    
    #Mayusculas a Minusculas
    rename 'y/A-Z/a-z/' *.ogg
    
    #Conversion de archivo *.ogg a *.wav
    for archivo in *.ogg; do oggdec $archivo; done
    
    #Comente la siguiente linea si no desea igualar el volumen de los ficheros
    normalize -m *.wav
    
    for archivo in *.wav; do
      #Variable auxiliar con el nombre base del archivo
      aux="$(basename "$archivo" .wav)"
      #Verificamos que el usuario introduzca el bitrate
      #En caso de no insertar el bitrate, se proporciona uno predeterminado
      if [ -z "$1" ]
      then
        echo ":Valor de bitrate no suministrado. Predeterminado: 160kbps."
        lame -b 160 "$aux.wav" "$aux.mp3"
      else
        lame -b $1 "$aux.wav" "$aux.mp3"
      fi
      #Verificamos posible errores
      #Si no hay errores, eliminamos el fichero *.wav
      if [ $? -eq 0 ]
      then
        rm -f "$aux.wav"
      fi
    done

**Nota:** Es posible que de acuerdo a la distribución que use, deba instalar ciertos paquetes, en mi caso solamente debi instalar el paquete `normalize` y el `lame`,  simplemente hice lo siguiente: `sudo apt-get install lame normalize`