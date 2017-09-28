---
redirect_from: "/archivos/2006/11/04/compilar-amsn-en-debian-etch-amd64/"
author: chantanito
comments: true
date: 2006-11-04 15:21:43
layout: post
slug: compilar-amsn-en-debian-etch-amd64
title: Compilar aMSN en Debian Etch AMD64
categories:
- debian
- GNU/Linux
- Software
tags:
- debian
- GNU/Linux
- Software
---

Bien, sin mucho preámbulo, lo primero que debemos hacer es [descargar el tarball](http://www.amsn-project.net/dlfile.php?file=amsn-0.96RC1.tar.bz2) de la [página de amsn](http://www.amsn-project.net/). Luego deberás descomprimirlo en la carpeta de tu preferencia, en mi caso está en ~/Sources/amsn-0.96RC1/. Una vez que lo descomprimes abre una terminal y obtén derechos de administrador (modo root); cuando tengas privilegios de root ubícate en el directorio donde descomprimiste el tarball y escribe lo siguiente:
  
	$ ./configure
	$ make
	$ make install

Debes asegurarte de cumplir todos los requisitos cuando haces el _./configure,_ ya que te pide varias "dependencias" por así decirlo, como por ejemplo, **tls** y **tk**. Una vez que hayas hecho el _make install_ quedará automágicamente instalado el amsn en tu sistema. Deberás poder verlo en Aplicaciones --> Internet --> aMSN. Bien eso es todo en lo que respecta al proceso de compilado, ¿nunca antes fué tan fácil verdad?.

Un problema que me dió una vez que lo compilé y lo ejecuté fué que no me permitía iniciar sesión porque me decía que no tenía instalado el módulo TLS. Entonces abrí una terminal e hice lo siguiente:

	$ aptitude install tcltls

...pero ésto no me solucionó el problema, entonces me puse a indagar por la web y me encontré con la siguiente solución: editar el archivo /usr/lib/tls1.50/**pkgIndex.tcl** y ubicar la línea que dice algo como: _package ifneeded tls 1.5_ para entonces modificarla por _package ifneeded tls 1.50_ y listo :D

