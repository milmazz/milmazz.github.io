---
redirect_from: "/archivos/2005/11/21/deskbar-applet-realiza-tus-busquedas-desde-el-escritorio/"
author: milmazz
comments: true
date: 2005-11-21 02:33:19
layout: post
slug: deskbar-applet-realiza-tus-busquedas-desde-el-escritorio
title: deskbar-applet, realiza tus búsquedas desde el escritorio
categories:
- Software
- Ubuntu
tags:
- Software
- Ubuntu
---

![deskbar-applet en funcionamiento](http://blog.milmazz.com.ve/wp-content/deskbar-applet.png) `deskbar-applet` es una de esas aplicaciones que parecen no tener mucho sentido en un principio, pero desde el mismo momento en que comienzas a utilizarla se te facilitan muchas actividades cotidianas.

`deskbar-applet` provee una versátil interfaz de búsqueda, incluso, puede abrir aplicaciones, ficheros, búsquedas locales (se integra complemente con [beagle](/article/2005/05/28/beagle/) si lo tienes instalado) o directamente en internet; aquellos términos que desee buscar, simplemente tendrá que escribirlos dentro de la casilla correspondiente en el panel. En caso de escribir una dirección de correo electrónico en la barra de búsqueda se le brindará la opción de escribir un correo al destinario que desee.

Si desea probarlo es muy sencilla su instalación. En primer lugar debe tener activa la sección `universe` en su lista de repositorios.

    deb http://us.archive.ubuntu.com/ubuntu breezy universe
    deb-src http://us.archive.ubuntu.com/ubuntu breezy universe

Una vez que haya editado el fichero `/etc/apt/sources.list` debe actualizar la nueva lista de paquetes.

    $ sudo aptitude update

Seguidamente puede proceder a instalar el paquete `deskbar-applet`, para ello simplemente haga.

    $ sudo aptitude install deskbar-applet

Una vez culminado el proceso de instalación debe activar `deskbar-applet` (esta aplicación aparece en la sección de _Accesorios_) para que aparezca en el panel que desee, recuerde que para agregar un elemento al panel simplemente debe hacer click con el botón derecho del _mouse_ y seleccionar la opción **Añadir al panel**.

Su uso es muy sencillo, posee una combinación de teclas (Alt + F3) que le facilitará enfocar la casilla de entrada, inmediatamente podrá comenzar a escribir.
