---
redirect_from: "/archivos/2006/05/29/feedjack-ya-esta-disponible/"
author: milmazz
comments: true
date: 2006-05-29 03:59:43
layout: post
slug: feedjack-ya-esta-disponible
title: 'FeedJack: Ya está disponible'
categories:
- Python
- Software
tags:
- django
- feedjack
- planet
- Python
- Software
---

Ya había comentado acerca de ésta aplicación en la entrada [FeedJack: Un Planet con esteroides](/archivos/2006/02/23/feedjack-un-planet-con-esteroides/), resulta que hace pocos días [Gustavo Picón](http://tabo.aurealsys.com/) [anunció la salida](http://tabo.aurealsys.com/archives/2006/05/27/release-feedjack-a-djangopython-powered-feed-aggregator/) de la versión [0.9.6](http://static.tabo.aureal.com.pe/code/Feedjack-0.9.6.tar.gz) bajo licencia **BSD**. Algunas de las características más resaltantes de esta aplicación son:

* Soporte de _virtual hosts_: Esto quiere decir que desde la misma instalación y la misma _Base de Datos_ e interfaz de administración, se manejan múltiples planetas. Si hay _feeds_ en común estos se bajan una sola vez para optimizar recursos.
* Temas (_Themes_): Cada sitio (_virtual host_) puede tener temas distintos.
* Soporte de folcsonomías (etiquetas o _tags_): Muy popular en las aplicaciones de la Web 2.0, se ha optimizado el algoritmo.
* Generación de páginas o _feeds_ por folcsonomías: [http://www.chichaplanet.org/tag/django/](http://www.chichaplanet.org/tag/django/)
* Generación de páginas o _feeds_ por miembros: [http://www.chichaplanet.org/user/1/](http://www.chichaplanet.org/user/1/)
* Generación de páginas o _feeds_ de una categoría en especial de un miembro particular: [http://www.chichaplanet.org/user/1/tag/django/](http://www.chichaplanet.org/user/1/tag/django/)
* Generar un _feed_ general: [http://www.chichaplanet.org/feed/](http://www.chichaplanet.org/feed/)
* Histórico de entradas: Entre muchas otras cosas. La principal ventaja frente a [Planet](http://planetplanet.org) es el soporte de un histórico de entradas o _posts_, por el uso de una Base de Datos.

Para conocer los detalles acerca de los requerimientos, el proceso de instalación, configuración y la modificación de los temas lea con detenimiento la entrada [Feedjack - A Django+Python Powered Feed Aggregator (Planet)](http://tabo.aurealsys.com/software/feedjack/).
