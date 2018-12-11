---
redirect_from: "/archivos/2007/05/14/recuperando-una-antigua-logitech-quickcam-express/"
author: milmazz
comments: true
date: 2007-05-14 21:45:53
layout: post
slug: recuperando-una-antigua-logitech-quickcam-express
title: Recuperando una antigua Logitech Quickcam Express
categories:
- debian
tags:
- debian
- GNU/Linux
- logitech
- Ocio
---

No se porque motivo o razón comencé a revisar en unas cajas de mi cuarto, cuando de repente me encontré con la primera cámara web que compre, de hecho, vino como accesorio a mi máquina de escritorio _Compaq Presario 5006LA_. Así que me pregunté, ¿será que todavía funciona esta reliquia?.

Lo primero que hice fue conectar el dispositivo en cuestión a mi portátil actual, enseguida ejecuté el comando:

    $ lsusb | grep -i logitech
    Bus 002 Device 002: ID 046d:0840 Logitech, Inc. QuickCam Express</code>

Una vez conocido el _PCI ID_ (046d:0840) del dispositivo realicé una búsqueda rápida en [Google](http://www.google.com) y llegué a un sitio muy interesante, en donde podemos obtener una descripción de los [dispositivos USB para Linux](http://www.qbik.ch/usb/devices/), al usar la función de [búsqueda en la base de datos](http://www.qbik.ch/usb/devices/search.php) del sitio mencionado previamente ingreso el dato correspondiente al **Vendor ID** (en mi caso, **046d**), posteriormente filtre los resultados por el **Product ID** (en mi caso, **0840**), sentía que ya estaba dando con la solución a mi problema, había encontrado información detallada acerca de mi [Logitech Quickcam Express](http://www.qbik.ch/usb/devices/showdev.php?id=2405). Al llegar acá descubrí el [Linux QuickCam USB Web Camera Driver Project](http://qce-ga.sourceforge.net/).

En la página principal del [Linux QuickCam USB Web Camera Driver Project](http://qce-ga.sourceforge.net/) observo que mi _vejestorio_ de cámara es soportada por el _driver_ `qc-usb`.

Con la información anterior decido hacer uso del manejador de paquetes `aptitude` y en los resultados avisté el nombre de un paquete `qc-usb-source`, así que **definitivamente** nuestra salvación es `module-assistant`.

    # aptitude install qc-usb-source \\
    build-essential \\
    module-assistant \\
    modconf \\
    linux-headers-`uname -r`
    # m-a update
    # m-a prepare
    # m-a a-i qc-usb

Una vez realizado el paso anterior recurro a la utilidad de configuración de módulos en [Debian](http://www.debian.org) `modconf` e instalo el módulo `quickcam``, el cual se encuentra en ``/lib/modules/2.6.18-4-686/misc/quickcam.ko` y verificamos.

    # tail /var/log/messages
    May 14 21:16:57 localhost kernel: Linux video capture interface: v2.00
    May 14 21:16:57 localhost kernel: quickcam: QuickCam USB camera found (driver version QuickCam USB 0.6.6 $Date: 2006/11/04 08:38:14 $)
    May 14 21:16:57 localhost kernel: quickcam: Kernel:2.6.18-4-686 bus:2 class:FF subclass:FF vendor:046D product:0840
    May 14 21:16:57 localhost kernel: quickcam: Sensor HDCS-1000/1100 detected
    May 14 21:16:57 localhost kernel: quickcam: Registered device: /dev/video0
    May 14 21:16:57 localhost kernel: usbcore: registered new driver quickcam

Como puede observarse el dispositivo es reconocido y se ha registrado en `/dev/video0`. En este instante que poseemos los módulos del driver `qc-sub` para nuestro _kernel_, podemos instalar la utilidad `qc-usb-utils`, esta utilidad nos permitirá modificar los parámetros de nuestra _Logitech QuickCam Express_.

    # aptitude install qc-usb-utils

Ahora podemos hacer una prueba rápida de nuestra cámara, comienza la diversión, juguemos un poco con `mplayer`.

    $ mplayer tv:// -tv driver=v4l:width=352:height=288:outfmt=rgb24:device=/dev/video0:noaudio -flip

A partir de ahora podemos probar más aplicaciones ;)
