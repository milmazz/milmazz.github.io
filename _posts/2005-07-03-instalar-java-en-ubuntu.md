---
redirect_from: "/archivos/2005/07/03/instalar-java-en-ubuntu/"
author: milmazz
comments: true
date: 2005-07-03 18:59:15
layout: post
slug: instalar-java-en-ubuntu
title: Instalar JAVA en Ubuntu
wordpress_id: 55
categories:
- Ubuntu
tags:
- Ubuntu
---

Debido a diversos motivos que no se expondrán en este artículo me he visto _obligado_ a desarrollar ciertas aplicaciones en JAVA, por todos es bien sabido que JAVA es un _formato restrictivo_, a pesar del formato abierto del API en sí, hasta ahora las únicas implementaciones de JAVA en GNU/Linux con una amplia compatibilidad se derivan de la implementación de [Sun Microsystem](http://www.sun.com/), ésta implementación lleva consigo _términos de licencias no libres_.

A pesar de la existencia de proyectos que buscan crear implementaciones **libres** de JAVA, estos aún no son comparables en rendimiento, acabado y compatibilidad con la implementación de Sun Microsystem.

Estos proyectos son:

  * [Kaffe.org](http://www.kaffe.org/)
  * [GNU Classpath](http://www.gnu.org/software/classpath/)

Así que procederé a describir el método más _elegante_ que he encontrado hasta ahora para instalar la implementación de JAVA de Sun Microsystem.

Obtenga la versión más reciente del fichero binario desde [la página de descargas de Sun](http://java.sun.com/j2se/1.5.0/download.jsp). Seleccione cualquiera de los enlaces de acuerdo a sus necesidades, ya sea para JDK o JRE. Recuerde que **JDK** soporta la creación de aplicaciones para plataforma de desarrollo J2SE, es decir, ideal para _desarrolladores_, mientras que **JRE** permite a los _usuarios finales_ ejecutar aplicaciones JAVA.

Una vez culminada la descarga, ejecute las siguientes sentencias:

    $ sudo apt-get install java-package fakeroot
    $ fakeroot make-jpkg jdk-1_5_0_02-linux-i586.bin
    $ sudo dpkg -i sun-j2sdk1.5_1.5.0+update02_i386.deb
    
Es importante aclarar que en las sentencias anteriores se asume que el paquete descargado ha sido el `jdk-1_5_0_02-linux-i586.bin`, evidentemente usted debe sustituir el nombre del paquete por el cual corresponda.

Si desea verificar la correcta instalación de JAVA, proceda de la siguiente manera:

    $ java -version

Después de la sentencia anterior usted debe recibir un mensaje similar al siguiente:

    java version "1.5.0_02"
    Java(TM) 2 Runtime Environment, Standard Edition (build 1.5.0_02-b09)
    Java HotSpot(TM) Client VM (build 1.5.0_02-b09, mixed mode, sharing)

Lo anterior <del>solo</del> ha sido probado bajo [Ubuntu Linux](http://www.ubuntulinux.org) versión [Hoary](http://www.ubuntulinux.org/504Released) y [Breezy](http://www.ubuntulinux.org/newsitems/release510). Una pequeña nota antes de culminar, en el caso de aparecerle el mensaje /java-web-start.applications: Permission denied mientras contruye el paquete `.deb`, no tiene mayor relevancia, puede ser ignorado.

## Nota para los usuario de Breezy

Si al ejecutar el comando `java -version` obtiene algo similar a lo mostrado a continuación:

    $ java -version
    java version "1.4.2"
    gij (GNU libgcj) version 4.0.2 20050808 (prerelease) (Ubuntu 4.0.1-4ubuntu9)
    
    Copyright (C) 2005 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

No se preocupe, simplemente cambie la versión de JAVA por omisión, para ello lea el artículo [Como cambiar entre versiones de JAVA bajo Breezy](/archivos/2005/11/28/como-cambiar-entre-versiones-de-java-bajo-breezy/).

## Referencias:

  * [Trampa JAVA](http://gnu.open-mirror.com/philosophy/java-trap.es.html) (español)
  * [JAVA, Formato Restrictivo](https://wiki.ubuntu.com/RestrictedFormats#head-55315677ab8f9890825549fa2ecebdde4bc68087) (inglés)
  * [Instalando Java 1.5](https://wiki.ubuntu.com/Java15) (inglés)
