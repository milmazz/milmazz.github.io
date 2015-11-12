---
redirect_from: "/archivos/2005/07/28/aptitude-aun-no-lo-usas/"
author: milmazz
comments: true
date: 2005-07-28 01:09:35
layout: post
slug: aptitude-aun-no-lo-usas
title: aptitude, ¿aún no lo usas?
wordpress_id: 61
categories:
- Software
- Ubuntu
tags:
- Software
- Ubuntu
---

Si usted es de los que piensa que aptitude por ser simplemente un [frontend](http://es.wikipedia.org/wiki/Frontend) de apt no puede aportar alguna ventaja al manejo óptimo de paquetes, trataré en lo posible en este artículo hacerle cambiar de parecer, o por lo menos mostrarle que en realidad aptitude si ofrece ciertas ventajas sobre apt.

La útil y avanzada herramienta que le permite manejar cómodamente los paquetes, apt (y dpkg) no lleva un registro (ni hace distinciones) de las aplicaciones que se instalan de manera explícita y las que se instalan de manera implícita por consecuencia del primer punto (es decir, para establecer la resolución de dependencias). Esta característica genera ciertos inconvenientes a la hora de desinstalar un paquete que posee dependencias que no son empleadas por otros programas, al momento de realizar la desinstalación lo más seguro es que no queden las cosas muy "limpias" en el sistema.

Un _remedio_ que en principio puede servirle es hacer uso de [deborphan](/archivos/2005/04/14/deborphan-eliminando-librerias-huerfanas/) (también puede hacer uso de [orphaner](http://linux.com.hk/PenguinWeb/manpage.jsp?section=8&name=orphaner), un frontend para deborphan); una herramienta de mayor potencia es [debfoster](http://www.fruit.eu.org/debfoster/), el primero de los mencionados busca librerias huérfanas (como se explico en un artículo anterior) y al pasarle estos resultados al apt-get remove se puede resolver de cierta manera el problema.

El problema de deborphan es que su campo de acción es limitado, por lo que la "limpieza" puede no ser muy buena del todo. En cambio debfoster si hace la distinción de la cual hablaba al principio de este artículo, los paquetes instalados de manera explícita y aquellos que son instalados de manera implícita para resolver las dependencias, por lo tanto debfoster eliminará no solamente las librerias huérfanas tal cual lo hace deborphan si no que también eliminará aquellos paquetes que fueron instalados de manera implícita y que actualmente ningún otro programa dependa de él, también serán eliminados en el caso en que se de una actualización y ya la dependencia no sea necesaria.

Ahora bien, se presenta otra alternativa, aptitude, este frontend de apt **si recuerda** las dependencias de un programa en particular, por lo que el proceso de remoción del programa se da correctamente. Ya anteriormente había mencionado que apt y dpkg **no** hacen distinción de las aplicaciones instaladas y Synaptic apenas lleva un histórico, esto en realidad no cumple con las espectativas para mantener un sistema bastante "limpio".

Aparte de lo mencionado previamente, otra ventaja que he encontrado en la migración a aptitude es que tienes dos opciones de manejo, la linea de comandos, la cual ha sido mi elección desde el comienzo, debido a la similitud de los comandos con los de apt y porque consigo lo que deseo inmediatamente, la interfaz gráfica no me llama la atención, pero quizás a usted si le guste. Adicionalmente, aptitude maneja de manera más adecuada el sistema de dependencias.

Para lograr ejecutar la interfaz gráfica de aptitude simplemente debe hacer:

    $ sudo aptitude

De verdad le recomiendo emplear aptitude como herramienta definitiva para el manejo de sus paquetes, primero, si es usuario habitual de apt, el cambio prácticamente no lo notará, me refiero al tema de la similitud de los comandos entre estas dos aplicaciones, segundo, no tendrá que estar buscando "remedios" para mantener limpio el sistema, aptitude lo hará todo por usted.
