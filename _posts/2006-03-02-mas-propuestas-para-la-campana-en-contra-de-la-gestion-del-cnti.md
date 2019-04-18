---
title: Más propuestas para la campaña en contra de la gestión del CNTI
author: milmazz
date: 2006-03-02 02:42:08
categories:
  - debian
  - Ubuntu
tags:
  - campaign
  - cnti
  - debian
  - Noticias
  - Scripts
  - software+libre
  - solve
  - Ubuntu
  - venezuela
slug: mas-propuestas-para-la-campana-en-contra-de-la-gestion-del-cnti
redirect_from: /archivos/2006/03/02/mas-propuestas-para-la-campana-en-contra-de-la-gestion-del-cnti/
---

[David Moreno Garza](http://damog.net/) (a.k.a. Damog) se ha unido a la campaña que apoya a la [Asociación de Software Libre de Venezuela (SoLVe)](http://solve.net.ve/), la cual [rechaza](http://www.abn.info.ve/go_news5.php?articulo=39148) (al igual que nosotros) el [acuerdo entre IBM Venezuela y el Centro Nacional de Tecnologías de Información](http://www.cnti.ve/cnti_docmgr/detalle.html?categoria=2987) ([CNTI](http://www.cnti.ve/)).

![MilMazz apoyo a SoLVe](http://blog.milmazz.com.ve/wp-content/uploads/2006/03/apoyo-solve-MilMazz.png) _Damog_ nos sorprende con un _script_ escrito en [Perl](http://www.perl.org/) que genera un botón personalizado con cierto mensaje.

Para la puesta en funcionamiento del _script_ necesitaremos en primera instancia instalar la variante `gd2` del módulo en Perl que contiene a la librería `libgd`, ésta última librería nos permite manipular ficheros PNG.

Tanto en Debian como en su hijo Ubuntu el procedimiento es similar al siguiente:

    $ sudo aptitude install libgd-gd2-perl
    $ wget http://www.damog.net/files/misc/apoyo-solve-0.1.zip
    $ unzip apoyo-solve-0.1.zip
    $ cd apoyo-solve-0.1
    $ perl apoyo-solve.perl <text>

En donde `<text>` debe ser reemplazado por su nombre o el de su sitio. Seguidamente proceda a subir la imagen.

Si lo desea, puede ver los diferentes [banners](http://www.conexionsocial.org.ve/wk/Decreto3390) de la campaña en contra de la gestión actual del CNTI, **únase al llamado de la Asociación de Software Libre de Venezuela (SoLVe)**.
