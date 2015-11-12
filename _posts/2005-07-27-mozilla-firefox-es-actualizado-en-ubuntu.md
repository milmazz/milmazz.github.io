---
author: milmazz
comments: true
date: 2005-07-27 03:20:37
layout: post
slug: mozilla-firefox-es-actualizado-en-ubuntu
title: Mozilla Firefox es actualizado en Ubuntu
wordpress_id: 60
categories:
- Software
- Ubuntu
tags:
- Software
- Ubuntu
---

Ciertos problemas han sido corregidos, como por ejemplo:




  * Problemas para instalar extensiones, a pesar de tener la última versión del paquete no se reconocia como tal, esto podía corregirse al modificarse la variable general.useragent.vendorSub y colocar como valor 1.0.4 en el about:config de [Firefox](http://www.mozilla.org/products/firefox/), ya no es necesario hacerlo.


  * Mozilla Firefox en algunas ocasiones podia generar una violación de segmento (y cerrarse de manera abrupta) simplemente por el hecho de cambiar entre pestañas, al cargar el manejador de extensiones o el historial.


  * Se corrigieron los fallos en la descarga de ciertos ficheros, por ejemplo, ficheros .torrent o .iso.



Estos problemas pueden apreciarse en la versión 5.04 (Hoary Hedgehog) de Ubuntu y los paquetes afectados son:



    
    <samp>mozilla-firefox
    mozilla-firefox-gnome-support</samp>




Se recomienda actualizar los paquetes afectados a la versión 1.0.6-0ubuntu0.1, recuerde que después de hacer la actualización debe reiniciar Firefox para que los cambios surtan efecto.




Puede ver en detalle el anuncio de la actualización en: [Fixed Firefox packages for USN-149-1](http://www.ubuntulinux.org/support/documentation/usn/usn-149-2).
