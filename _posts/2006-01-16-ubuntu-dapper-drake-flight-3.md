---
title: Ubuntu (Dapper Drake) Flight 3
author: milmazz
date: 2006-01-16 22:04:35
categories:
  - Ubuntu
tags:
  - dapper
  - linux
  - Ubuntu
slug: ubuntu-dapper-drake-flight-3
redirect_from: /archivos/2006/01/16/ubuntu-dapper-drake-flight-3/
---

La tercera versión _alpha_ de Ubuntu 6.04 (Dapper Drake), continúa mostrando mejoras e incluye nuevo software.

Las mejoras incluyen una actualización en el tema, el cual desde la segunda versión _alpha_ es manejado por `gfxboot`.

![gfxboot theme splash](http://www.simplifiedcomplexity.com/images/screenshots/dapper/flight3/gfxboot-theme-splash-small.png)

Ademas se incluye X Window System versión `X11R7`, GNOME `2.13.4`, también se observan mejoras y simplificación de los menús, algunas nuevas aplicaciones como XChat-GNOME, un LiveCD más rápido y que permite almacenar nuestras configuraciones.

También se notan algunas mejoras estéticas en el cuadro de dialógo de cierre de sesión.

![Log Out Screen](http://www.simplifiedcomplexity.com/images/screenshots/dapper/flight3/logout.png)

En cuanto a la mejora y simplificación de los menús, la idea básicamente es obviar aquellas opciones que pueden llegar a ser confusas para los usuarios, también se evita la duplicación de opciones, esto permite que exista un solo punto para acceder a cada función del sistema, mejorando de esta manera la usabilidad en nuestro escritorio favorito.

Se ha creado un nuevo dialógo que indica cuando es necesario reiniciar el sistema, esto sucede cuando se realizan importantes actualizaciones al sistema, en donde es recomendable reiniciar el sistema para que dichas actualizaciones surtan efecto.

![restart bubble](http://www.simplifiedcomplexity.com/images/screenshots/dapper/flight3/restart-bubble.png)

## ¿Qué mejoras incluye la versión LiveCD?

Quién haya usado alguna vez en su vida un LiveCD puede haberse percatado que éstos presentan ciertos problemas, uno de ellos es la lentitud en el tiempo de carga del sistema, en este sentido se han realizado algunas mejoras en el cargador utilizado en el arranque, el tiempo de carga se ha reducido aproximadamente de unos 368 segundos a 231 segundos, esta mejora es bastante buena, aunque se espera mejorar aún mas este tiempo de carga del LiveCD.

Otro de los problemas encontrados en los LiveCD, es que el manejo de los datos no es persistente, esta nueva versión incluye una mejora que permite recordar las configuraciones, esto quiere decir que la siguiente vez que usted utilice el LiveCD dichas configuraciones serán recordadas. Esto es posible ya que el LiveCD permite guardar sus cambios en un dispositivo externo (al CD) como por ejemplo un _llavero usb_. Por lo tanto, si usted especifica el parámetro `persistent` cuando usted esta iniciando el LiveCD, éste buscará el dispositivo externo que mantiene las configuraciones que usted ha almacenado. Si desea conocer más acerca de esta nueva funcionalidad en el LiveCD vea el documento [LiveCDPersistence](https://wiki.ubuntu.com/LiveCDPersistence).

Si usted desea descargar esta tercera versión _alpha_ de Ubuntu 6.04, puede hacerlo en [Ubuntu (Dapper Drake) Flight CD 3](http://cdimage.ubuntu.com/releases/dapper/flight-3/).

Mayor detalle acerca de las nuevas características que presenta esta nueva versión en el documento [DapperFlight3](https://wiki.ubuntu.com/DapperFlight3).

**Nota:** Esta versión no es recomendable instalarla en entornos de producción, la versión estable de Dapper Drake se espera para Abril de este mismo año.
