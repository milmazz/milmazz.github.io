---
redirect_from: "/archivos/2007/07/08/transmission-072-en-debian-y-ubuntu-gnulinux-amd64/"
author: chantanito
comments: true
date: 2007-07-08 17:52:20
layout: post
slug: transmission-072-en-debian-y-ubuntu-gnulinux-amd64
title: Transmission 0.72 en Debian y Ubuntu GNU/Linux AMD64
categories:
- debian
- GNU/Linux
- Software
- Ubuntu
tags:
- bittorrent
- debian
- Distribuciones
- General
- GNU/Linux
- Internet
- Software
- transmission
- Ubuntu
---

Bien, en realidad, no he podido esperar a tenerlo trabajando al 100%, se trata de la versión 0.72 de [Transmission](http://transmission.m0k.org/about.php), el que a mi parecer, es el mejor cliente [BitTorrent](http://es.wikipedia.org/wiki/BitTorrent) que jamás haya existido. Según lo describen en la página, cito textualmente:

> Transmission has been built from the ground up to be a lightweight, yet powerful BitTorrent client. Its simple, intuitive interface is designed to integrate tightly with whatever computing environment you choose to use. Transmission strikes a balance between providing useful functionality without feature bloat. Furthermore, it is free for anyone to use or modify.

Su instalación es **muy fácil**, ya que lo único que tenemos que hacer, es bajarnos el .deb (sí, el .deb, imagínense lo fácil que nos va a resultar) de la página de nuestros amígos de [GetDeb](http://www.getdeb.net/app.php?name=Transmission) y luego usar una terminal ó el instalador de paquetes GDebi (aún más fácil) para instalar el paquete.

En el primer de los casos, usando la terminal, lo único que tenemos que hacer en escribir la siguiente línea de comandos:
`$ sudo dpkg -i transmission_0.72-0~getdeb1_amd64.deb`

``
...esperar a que termine el proceso de instalación y ya podrás ejecutar el Transmission desde _Aplicaciones --> Internet --> Transmission_.

Para el segundo de los casos, usando el instalador GDebi, tan sólo hay que hacer click encima del .deb con el botón derecho del ratón y seleccionamos la opción _Abrir con "Instalador de paquetes GDebi"_ y luego click en el botón _Instalar el paquete_, finalmente esperar a que finalice la instalación del paquete y listo!. Una de las cosas que, debo admitir, más me gusta de ésta nueva versión, es que ahora podemos minimizar la aplicación en la bandeja del sistema :) .

Para más información de Transmission, visite su [Página Oficial](http://transmission.m0k.org/index.php).
