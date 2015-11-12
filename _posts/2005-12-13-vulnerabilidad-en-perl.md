---
redirect_from: "/archivos/2005/12/13/vulnerabilidad-en-perl/"
author: milmazz
comments: true
date: 2005-12-13 01:50:29
layout: post
slug: vulnerabilidad-en-perl
title: Vulnerabilidad en Perl
wordpress_id: 120
categories:
- Seguridad
- Ubuntu
tags:
- Perl
- Seguridad
- Ubuntu
---

Las siguientes versiones se encuentran afectadas ante este fallo de seguridad:

  * Ubuntu 4.10 (Warty Warthog)
  * Ubuntu 5.04 (Hoary Hedgehog)
  * Ubuntu 5.10 (Breezy Badger)

En particular, los siguientes paquetes se encuentran afectados:

  * `libperl5.8`
  * `perl-base`

El problema puede ser corregido actualizando los paquetes a sus últimas versiones en las respectivas versiones de Ubuntu. En general, el modo estándar de actualizar la distribución será mas que suficiente.
    
    $ sudo aptitude dist-upgrade

La actualización pretende solucionar una vulnerabilidad del interprete [Perl](http://www.perl.org/), el cual no era capaz de manejar todos los posibles casos de una entrada malformada que podría permitir la ejecución de código arbitrario, así que es **recomendable** actualizar su sistema de inmediato.

Sin embargo, es importante hacer notar que esta vulnerabilidad **puede** ser aprovechada en aquellos programas _inseguros_ escritos en Perl que utilizan variables con valores definidos por el usuario en cadenas de caracteres y en donde no se realiza una verificación de dichos valores.

Si desea mayor detalle, le recomiendo leer el anuncio hecho por Martin Pitt en [[USN-222-1] Perl vulnerability](http://lists.ubuntu.com/archives/ubuntu-security-announce/2005-December/000250.html).
