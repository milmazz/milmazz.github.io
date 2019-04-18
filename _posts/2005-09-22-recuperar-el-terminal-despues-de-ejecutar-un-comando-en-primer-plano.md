---
title: Recuperar el Terminal después de ejecutar un comando en primer plano
author: milmazz
date: 2005-09-22 15:29:38
tags:
  - General
slug: recuperar-el-terminal-despues-de-ejecutar-un-comando-en-primer-plano
redirect_from: /archivos/2005/09/22/recuperar-el-terminal-despues-de-ejecutar-un-comando-en-primer-plano/
---

Si regularmente ejecuta programas desde el **Terminal**, seguramente en alguna
ocasión habrá olvidado añadirle al final del comando un carácter `&`, por si no
lo sabe, el uso del carácter `&` permite ejecutar el comando en segundo plano,
por lo tanto, podrá seguir utilizando el **Terminal** para llevar a cabo otras
actividades.

Si por casualidad se encuentra ante la situación descrita en el párrafo
anterior, existe una solución rápida  y efectiva, después de ejecutar el comando
desde el Terminal sin el carácter `&` presione la combinación de teclas Ctrl + Z
dentro del mismo Terminal, esto permitirá detener (más **no** cancelar) el
programa que ha ejecutado previamente, para reanudar la ejecución del programa
use el comando `bg`.

Veamos un ejemplo para aclarar las posibles dudas.

    milmazz@omega:~$ gaim

    [1]+  Stopped                 gaim
    milmazz@omega:~$ bg
    [1]+ gaim &

El comando `bg` permite ejecutar el programa que se encontraba detenido en
segundo plano, es decir, como si desde el principio hubiese ejecutado el
programa de la siguiente manera:

    $ gaim &

Este _tip_ me ha servido en muchas ocasiones, espero lo encuentre útil al igual
que yo.
