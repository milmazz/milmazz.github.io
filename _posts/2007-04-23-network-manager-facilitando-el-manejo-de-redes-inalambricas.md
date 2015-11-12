---
redirect_from: "/archivos/2007/04/23/network-manager-facilitando-el-manejo-de-redes-inalambricas/"
author: milmazz
comments: true
date: 2007-04-23 23:50:08
layout: post
slug: network-manager-facilitando-el-manejo-de-redes-inalambricas
title: 'Network Manager: Facilitando el manejo de redes inalámbricas'
wordpress_id: 213
categories:
- debian
- GNU/Linux
tags:
- debian
- GNU/Linux
- network-manager
- wireless
---

![NetworkManager](/images/2007-04-23-network-manager-facilitando-el-manejo-de-redes-inalambricas/network-manager.thumbnail.png) En la entrada previa, [Establecer red inalámbrica en Dell m1210](/archivos/2007/04/21/establecer-red-inalambrica-en-dell-m1210/), comencé a describir el proceso que seguí para lograr hacer funcionar la tarjeta _Broadcom Corporation Dell Wireless 1390 WLAN Mini-PCI Card (rev 01)_ en una portátil _Dell m1210_. El motivo de esta entrada se debe a que muchos usuarios hoy día no les interesa ni debe interesarles estar lidiando con la detección de redes inalámbricas, por eso les pasaré a comentar acerca de [NetworkManager](http://www.gnome.org/projects/NetworkManager/).

**NetworkManager** es una aplicación cuyo objetivo es que el usuario nunca tenga que lidiar con la línea de comandos o la edición de ficheros de configuración para manejar sus redes (ya sea cableada o inalámbrica), haciendo que la detección de dichas redes _simplemente funcione_ tanto como se pueda y que interrumpa lo menos posible el flujo de trabajo del usuario. De manera que cuando usted se dirija a áreas en las cuales usted ha estado antes, **NetworkManager** se conectará automáticamente a la última red que haya escogido. Asimismo, cuando usted esté de vuelta al escritorio, **NetworkManager** cambiará a la red cableada más rápida y confiable.

Por los momentos, **NetworkManager** soporta redes cifradas WEP, el soporte para el cifrado WPA está contemplado para un futuro cercano. Respecto al soporte de VPN, **NetworkManager** soporta hasta ahora vpnc, aunque también está contemplado darle pronto soporte a otros clientes.

Para hacer funcionar **NetworkManager** en Debian los pasos que debemos seguir son los siguientes. En primera instancia instalamos el paquete.
    
    # aptitude -r install network-manager-gnome

Que conste que **NetworkManager** funciona para entornos de escritorios como [GNOME](http://gnome.org), [KDE](http://kde.org), [XFCE](http://xfce.org), entre otros. En este caso particular estoy instalando el paquete disponible en Debian para GNOME en conjunto con sus recomendaciones.

De acuerdo al fichero `/usr/share/doc/network-manager/README.Debian` **NetworkManager** consiste en dos partes: uno a nivel del demonio del sistema que se encarga de manejar las conexiones y recoge información acerca de las nuevas redes. La otra parte es un _applet_ que el usuario emplea para interactuar con el demonio de **NetworkManager**, dicha interacción se lleva a cabo a través de [D-Bus](http://www.freedesktop.org/wiki/Software/dbus).

En Debian por seguridad, los usuarios que necesiten conectarse al demonio de **NetworkManager** deben estar en el grupo `netdev`. Si usted desea agregar un usuario al grupo `netdev` utilice el comando `adduser usuario netdev`, luego de ello tendrá que recargar `dbus` haciendo uso del comando `/etc/init.d/dbus reload`.

Es necesario saber que **NetworkManager** manejará todos aquellos dispositivos que **no** estén listados en el fichero `/etc/network/interfaces`, o aquellos que estén listados en dicho fichero con la opción `auto` o `dhcp`, de esta manera usted puede establecer una configuración para un dispositivo que sea estática y puede estar seguro que **NetworkManager** no tratará de sobreescribir dicha configuración. Para mayor información le recomiendo leer detenidamente el fichero `/usr/share/doc/network-manager/README.Debian`.

Si usted desea que **NetworkManager** administre todas las interfaces posibles en su ordenador, lo más sencillo que puede hacer es dejar solo lo siguiente en el fichero `/etc/network/interfaces`.
  
    $ cat /etc/network/interfaces
    auto lo
    iface lo inet loopback

Una vez que se ha modificado el fichero `/etc/network/interfaces` reiniciamos **NetworkManager** con el comando `service network-manager restart`. El programa ahora se encargará de detectar las redes inalámbricas disponibles. Para ver una lista de las redes disponibles, simplemente haga clic en el icono, tal como se muestra en la figura al principio de este artículo.

Había mencionado previamente que **NetworkManager** se conectará automáticamente a las redes de las cuales tiene conocimiento con anterioridad, pero usted necesitará conectarse manualmente a una red al menos una vez. Para ello, simplemente seleccione una red de la lista y **NetworkManager** automáticamente intentará conectarse. Si la red requiere una llave de cifrado, **NetworkManager** le mostrará un cuadro de dialogo en el cual le preguntará acerca de ella. Una vez ingresada la llave correcta, la conexión se establecerá.

Para cambiar entre redes, simplemente escoja otra red desde el menú que le ofrece el _applet_.
