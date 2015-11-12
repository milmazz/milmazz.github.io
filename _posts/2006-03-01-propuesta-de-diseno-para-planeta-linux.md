---
redirect_from: "/archivos/2006/03/01/propuesta-de-diseno-para-planeta-linux/"
author: milmazz
comments: true
date: 2006-03-01 14:30:58
layout: post
slug: propuesta-de-diseno-para-planeta-linux
title: Propuesta de diseño para Planeta Linux
wordpress_id: 145
categories:
- GNU/Linux
tags:
- (X)HTML
- bluefish
- CSS
- design
- gedit
- GIMP
- GNU/Linux
- icons
- inkscape
- layout
- planet
- planetalinux
---

En estos últimos días he estado trabajando en [mi propuesta de diseño](http://www.ubuntuchannel.org/pruebas/planeta/) para [Planeta Linux](http://www.planetalinux.org), sitio del cual escribí previamente en la [entrada acerca de Planeta Linux](/archivos/2006/02/07/planeta-linux/), hasta ahora considero que está bastante avanzado el diseño.

A continuación, algunos detalles acerca del por qué y el cómo del trabajo realizado hasta ahora.

## La estructura del diseño

La estructura del diseño es bastante sencilla, consiste en 3 partes:

  * Cabecera
  * Contenido
  * Pie de página

Hasta ahora el _pie de página_ presenta cierta _redundancia de datos_, puesto que la documentación que se ubicará en la página principal aún no está completa (que conste que _no_ es una medida de presión para Damog).

El modo en el cual se presenta la información pretende darle **prioridad** al contenido expuesto por cada uno de los miembros del Planeta Linux, facilitándole la tarea al lector, es decir, encontrar la información que él desea en el menor tiempo posible, esto es de suma importancia en estos tiempos tan _agitados_.

Se le ha agregado cierta presencia al _pie de página_, pero no tanta como a la sección de _contenido_; la funcionalidad del _pie de página_ ahora reemplaza al uso de la común _barra lateral_. Regularmente el _pie de página_ es ocupado solo para mostrar notas sin mayor relevancia y el _copyright_ que aplique, en esta propuesta, las cosas cambian un poco.

## ¿Qué herramientas he utilizado?

Todo el trabajo se ha realizado usando herramientas libres, entre ellas sobresalen:

  * [Inkscape](http://www.inkscape.org/)
  * [The GIMP!](http://www.gimp.org/)
  * [Bluefish](http://bluefish.openoffice.nl/index.html)
  * [Gedit](http://www.gnome.org/projects/gedit/)

## Respecto a los iconos

Los iconos que he usado hasta ahora se encuentran en la serie [Silk](http://www.famfamfam.com/lab/icons/silk/), la cual cuenta con más de **700** iconos de 16x16 _pixels_ en formato PNG, estos iconos están bajo la licencia [Creative Commons Attribution 2.5 License](http://creativecommons.org/licenses/by/2.5/).

Para las banderas que se muestran en la barra de navegación de la cabecera, he usado la serie [Flag](http://www.famfamfam.com/lab/icons/flags/), ésta serie cuenta con **239** banderas, tanto en formato GIF como PNG, se pueden utilizar libremente para cualquier propósito.

También he utilizado el _set de íconos [nuoveXT](http://nuovext.pwsp.net/), el cual provee una completa gama de iconos tanto para GNOME como KDE._

Ya para finalizar la sección de los iconos, Damog hizo bien al recomendarme el set de iconos propuesto en [Tango Desktop Project](http://tango-project.org/).

## TODO (cosas por hacer)


  * Hacer que el contenido se ajuste a un determinado **porcentaje** de la resolución de la pantalla, para aprovechar el espacio en resoluciones muy altas.
  * <del>Eliminar el efecto _scroll_</del>. Se ha eliminado en la versión 0.2
  * Mejorar la barra de navegación de la cabecera.
  * Diseñar un logo decente, quizá [Jonathan Hernández](http://ion.gluch.org.mx/) (líder del proyecto [Jaws](http://www.jaws-project.com/)) nos ayude en esto, según contaba [Damog](http://www.damog.net) en la lista de correos de Planeta Linux.
  * ¿Sería conveniente colocar el URI del _feed_ del autor correspondiente en su campo de información?, la cual está ubicada antes de cada _globo de dialógo_.

Por supuesto, cualquier actividad que haga falta será añadida a la lista de _cosas por hacer_

Cualquier sugerencia, corrección, comentario es bienvenido.