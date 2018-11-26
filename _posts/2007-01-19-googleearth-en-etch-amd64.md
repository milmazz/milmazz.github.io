---
redirect_from: "/archivos/2007/01/19/googleearth-en-etch-amd64/"
author: chantanito
comments: true
date: 2007-01-19 10:39:14
layout: post
slug: googleearth-en-etch-amd64
title: GoogleEarth en Etch AMD64
tags:
- General
---

Hace días pude darme cuenta que el [paquete](http://packages.debian.org/testing/misc/googleearth-package) para instalar [Google Earth](http://earth.google.es/) en [Debian](http://www.debian.org) se encuentra en los repositorios (en la sección [Contrib]). Debo detenerme un momento acá para mencionar que el paquete [googleearth-package](http://packages.debian.org/testing/misc/googleearth-package) **es Software Libre**, sin embargo, depende de Google Earth que es total y absolutamente **Software Propietario **, por lo tanto deberás estár dispuesto a "ensuciar" un poco tu Debian.

Bien, hasta ahora sólo he mencionado que el paquete está en los repositorios bla bla bla, pero no he mencionado algo MUY importante, y es que, éste paquete (googleearth-package) no hace nada más que adornar (al menos por los momentos) los repositorios, ya que si se ubican en la sección de [ descargas de Google Earth ](http://earth.google.es/download-earth.html)se podrán dar cuenta que no existen binarios para la arquitectura AMD64, por lo cual tendrán que [ crear un chroot](/article/2006/11/22/creando-un-chroot-en-etch-amd64/) para poder correr la versión disponible (la de 32bits). Otra cosa MUY pero MUY importante que debo hacer es recomendarles que, por favor, le echen un vistazo a los requerimiento mínimos para que no tengan una mala experiencia en lo que a ejecutar el Google Earth refiere.

Una vez que han creado el chroot, el primer paso sería instalar el paquete googleearth-packages, lo cual se puede lograr con la siguiente línea de comandos en una terminal:

     # aptitude install googleearth-package

Ésto también lo pueden hacer por el Gestor de Paquetes Synaptic sin ningún problema, es cuestión de gustos y costumbres. Cuando vayan a instalar el googleearth-package, el apt posiblemente les sugiera instalar también el paquete fakeroot, ésta sugerencia deberán aceptarla ya que, el fakeroot es necesario para poder construir el .deb de google-earth que vamos a instalar; él (fakeroot) básicamente lo que hace es quitar la necesidad de trabajar como root para poder construir paquetes. También el apt puede hacerle otras muchas sugerencias, sobre todo si el chroot está recién creado.

Una vez que se haya instalado todo, procedemos a descargar el binario de Google Earth de la página. Éste paso queda a juicio del lector, ya que puede descargarlo como se le de la antojada gana; en lo personal, lo hice utilzando la utilidad wget (Debian definitely rulez!) con la siguiente línea de comandos:

     # wget http://dl.google.com/earth/GE4/GoogleEarthLinux.bin

Por cierto, ésa la URL para la descarga :).

Cuando haya terminado la descarga (el binario ha de "pesar" unos 20 Mb aproximadamente) lo siguiente que hacemos es construir el paquete, lo cual logramos con un simple:

     # make-googleearth-package

Y básicamente ése es todo el proceso para construir el .deb. Lo único que faltaría sería instalarlo (casi nada jeje), pero eso es tan fácil como hacer en la terminal:

    # dpkg -i googleearth_4.0.2723.0-1_i386.deb

Cuando ejecutes Google Earth el programa quizá te llame la atención diciéndote que no tienes instalada la fuente Bitstream Vera, pero ésto no es problema alguno ya que el programa corre perfectamente (al menos en mi caso :)); de todas formas para instalar la fuente es tan simple como hacer en una terminal:

     # aptitude install ttf-bitstream-vera

En éste momento ya debería estar todo instalado en su máquina. Que lo disfruten! Aquí un vistazo del Google Earth corriendo en mi PC:

![Google Earth en mi Etch AMD64](http://farm1.static.flickr.com/140/362524891_e3d67a633f.jpg?v=0)
