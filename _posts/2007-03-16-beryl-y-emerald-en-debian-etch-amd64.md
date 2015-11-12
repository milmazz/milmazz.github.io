---
redirect_from: "/archivos/2007/03/16/beryl-y-emerald-en-debian-etch-amd64/"
author: chantanito
comments: true
date: 2007-03-16 18:55:50
layout: post
slug: beryl-y-emerald-en-debian-etch-amd64
title: Beryl y Emerald en Debian "Etch" AMD64
wordpress_id: 211
categories:
- debian
- GNU/Linux
- Software
tags:
- beryl
- debian
- emerald
- General
- GNU/Linux
- Ocio
- Software
---

Sin mucho preámbulo, sólo tengo que decir que voy explicar cómo tener instalado éste famoso escritorio 3D ([Beryl](http://www.beryl-project.org/)) en nuestros sistemas [Debian](http://www.debian.org) AMD64. El proceso en general es muy fácil y se resume en unos pocos pasos. Antes que nada debo mencionar que la placa de video que uso es [nVIDIA](http://www.nvidia.com) y que para poder utilizar el Beryl hay que hacer [ciertas modificaciones](http://wiki.beryl-project.org/wiki/Install_Beryl_on_Debian#XORG.CONF) al **xorg.conf**.

Lo primero que debemos hacer es modificar nuestro **/etc/apt/sources.list** para añadir una nueva entrada que va a ser el servidor desde donde se van a instalar **Beryl** y **Emerald**. Ésto lo logramos con la siguiente línea de comandos en una terminal:

     # vim /etc/apt/sources.list

La línea que vamos a agregar a nuestro _sources.list_ es la que se corresponde con el [repositorio de Beryl para Debian](http://debian.beryl-project.org/) y es la siguiente:
 
    deb http://debian.beryl-project.org/ etch main

Luego, como han de sospechar, hay que actualizar la base de datos del aptitude, lo cual se logra así:

    # aptitude update

Una vez actualizada la base de datos procedemos a instalar el Beryl con la siguente línea de comandos:

    # aptitude install -ry beryl

Ésta última línea nos va a instalar el Beryl automáticamente con todos los paquetes recomendados y va a asumir "Sí" como respuesta para poder realizar la instalación. Una vez que está instalado podremos ejecutarlo desde _Aplicaciones --> Herramientas del sistema --> Beryl Manager_ y los temas del Emeral los podemos seleccionar en _Escritorio --> Preferencias --> Emerald Theme Manager_. Las opciones del Beryl pueden ser modificadas con el Beryl Settings Manager, el cual puede ser localizado en la misma ruta que el Beryl Manager.
Si queremos que el Beryl Manager sea ejecutado cada vez que iniciamos sesión debemos añadirlo a la lista de programas al inicio. Ésto lo hacemos ejecutando _Escritorio --> Preferencias --> Sesiones _ y en la pestaña _Programas al inicio_ hacemos click en _Añadir_ y escribimos **beryl-manager**.
A continuación algunos atajos del teclado para lograr los efectos más comunes:

  1. Modo de movimiento de imagen borrosas = Ctrl + F12
  2. Rotar escritorios como un cubo = Ctrl + Alt + Flechas direccionales
  3. Efecto de lluvia = Shift + F9
  4. Zoom = Super + Scroll
  5. Selector de ventanas escalar = Super + Pausa
  6. Rotar ventana entre espacios de trabajo con el cubo = Ctrl + Alt + Shif + Teclas direccionales
  7. Modificar la opacidad de la ventana actual = Alt + Scroll

Por último [un artículo](http://gskbyte.wordpress.com/2007/02/04/lo-que-viene-con-beryl-02/) donde explican las virtudes del Beryl 0.2 y dos videos, que a mi criterio son las mejores demostraciones de Beryl que jamas haya visto.
