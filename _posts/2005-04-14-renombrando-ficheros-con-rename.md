---
title: Renombrando ficheros con rename
author: milmazz
date: 2005-04-14 02:04:15
categories:
  - GNU/Linux
tags:
  - GNU/Linux
slug: renombrando-ficheros-con-rename
redirect_from: /archivos/2005/04/14/renombrando-ficheros-con-rename/
---

Si ud. es una de esas personas que al igual que yo le gusta ahorrar tiempo en
esas tareas cotidianas  que a veces se convierten en algo aburridas por lo
repetitivas que suelen ser, seguramente este breve artículo acerca del uso de
`rename` le va a interesar, puedo asegurarle que le facilitará la vida, claro,
siempre y cuando ud. regularmente modifique nombres de ficheros. Seguramente se
ha encontrado alguna vez que el formato de nombres de los ficheros de audio
(p.ej. mp3) no le parece el adecuado, en estas situaciones lo más recomendable
es recurrir a `rename`, ya que éste último le automátizara el renombrado de los
ficheros al indicarle un parámetro, el cual será una expresión regular de Perl.
Algunos ejemplos de esto son los siguientes:

    milton@omega ~ $ ls
    01 - Main Theme From Star Wars & Leia's Nightmare.mp3
    milton@omega ~ $ rename 's/\ -\ /-/' *.mp3

Obtenemos como resultado lo siguiente:

    milton@omega ~ $ ls
    01-Main Theme From Star Wars & Leia's Nightmare.mp3

Como pudo haberse dado cuenta el comando `rename 's/\ -\ /-/' *.mp3` simplemente
sustituyó la **cadena** " - " por '-', recuerde que los espacios en sistemas
**GNU/Linux** son "carácteres especiales", por lo tanto, al carácter debe
precederle una barra invertida (**\**), los ficheros que modificará son aquellos
que posean la extensión **mp3**.

También puede notar que en el nombre del fichero anterior aún existen espacios
en blanco, si deseamos convertir dichos espacios en blanco por el carácter '_',
simplemente realizamos lo siguiente:

    milton@omega ~ $ rename 'y/\ /_/' *.mp3<br></br>
    milton@omega ~ $ ls<br></br>
    01-Main_Theme_From_Star_Wars_&_Leia's_Nightmare.mp3

Como puede darse cuenta, el comando `rename 'y/\ /_/' *.mp3` **sustituirá**
todos los carácteres que representan los espacios en blanco por el carácter '-'
en todos los nombres ficheros con extensión **mp3**.

Otra posibilidad interesante que tenemos es la de convertir todos los carácteres
en mayúsculas a mínusculas y viceversa.

    milton@omega ~ $ ls
    10_MOS_EISLEY_SPACEPORT.mp3
    milton@omega ~ $ rename -v 'y/A-Z/a-z/' *.mp3
    10_MOS_EISLEY_SPACEPORT.mp3 renamed as 10_mos_eisley_spaceport.mp3

Como vemos, el cambio es realmente sencillo, al utilizar la opción **-v**
simplemente estamos habilitando la impresión de los nombres de los archivos que
se han modificado correctamente, esta opción es bastante útil para percatarnos
de los cambios que se han generado.

Este tratamiento también es aplicable para renombrar directorios, recuerde que
los directorios en sistemas GNU/Linux son tratados de manera equivalente a los
archivos, así que vamos a mostrar un ejemplo para aclarar la situación.

    milton@omega ~ $ ls
    Shadows of the Empire
    Star Wars A New Hope
    Star Wars Attack Of The Clones
    Star Wars Return of the Jedi
    Star Wars The Empire Striks Back
    Star Wars The Phantom Menace

Arriba vemos los directorios que representan la mayoría de los álbumes de la
colección de **Star Wars**, por comodidad, regularmente suelo hacer una
estructura similar a la siguiente: Music » Artista » Título del Álbum »
#Pista-Nombre de la Pista. Por lo que los **títulos de los álbumes** deben ser
especificos, no deben contener el **nombre del artista**. En mi caso he cambiado
los nombres de los directorios de la siguiente manera:

    milton@omega ~ $ rename -v 's/Star\ Wars\ //' *
    Star Wars A New Hope renamed as A New Hope
    Star Wars Attack Of The Clones renamed as Attack Of The Clones
    Star Wars Return of the Jedi renamed as Return of the Jedi
    Star Wars The Empire Striks Back renamed as The Empire Striks Back
    Star Wars The Phantom Menace renamed as The Phantom Menace

En el ejemplo anterior simplemente se ha eliminado la cadena "Star Wars ". Es de
suponer que el directorio **Shadows of the Empire** no será renombrado ya que no
cumple con las pautas necesarias.

Es importante aclarar, que éste no es el único método existente para la
modificación de los nombres de los ficheros, existen muchos otros, pero desde mi
punto de vista, el uso de `rename` me parece bastante adecuado y fácil de
implantar.
