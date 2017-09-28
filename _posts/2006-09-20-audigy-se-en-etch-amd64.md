---
redirect_from: "/archivos/2006/09/20/audigy-se-en-etch-amd64/"
author: chantanito
comments: true
date: 2006-09-20 02:29:28
layout: post
slug: audigy-se-en-etch-amd64
title: Audigy SE en Etch AMD64
categories:
- debian
- GNU/Linux
tags:
- debian
- GNU/Linux
---

Hace días me hice con una tarjeta de sonido [Audigy SE](http://creative.com/products/product.asp?category=1&subcategory=205&product=14257) de la marca [Creative](http://www.creative.com/) y la verdad tuve que dar bastante golpes para dar con la configuración correcta. ¿Cómo lo logré? Muy fácil: Googleando :).

Resulta que el sistema ([Debian](http://www.debian.org), of course) reconoce bien el dispositivo y carga de manera perfecta los módulos necesarios; de hecho el alsa-mixer me mostraba todos los canales de la tarjeta pero el único que parecía funcionar era el _Analog Front_ y los otros parecían estar muertos. Casi daño el mouse de tando hacer clicks y darle hacia arriba y hacia abajo :). No podía creer que no funcionara!.

Googleando llegué a la página de la gente de [Alsa](http://www.alsa-project.org/) y me encontré [una manera](http://www.alsa-project.org/alsa-doc/doc-php/template.php?company=Creative+Labs&card=Sound+Blaster+Audigy+SE.&chip=CA0106&module=ca0106) de configurar los drivers. Ahi confirmé que el sistema me estaba cargando el módulo correcto, el [snd-ca0106](http://www.alsa-project.org/alsa-doc/doc-php/template.php?company=Creative+Labs&card=Sound+Blaster+Audigy+SE.&chip=CA0106&module=ca0106). Entonces, después de ver la pequeña lista de instrucciones, no voy a decir que me espanté y maldije a Creative, no, no lo hice, en ése momento miré al cielo y pregunté: ¡¿Acaso no existe una manera más fácil de lograr que suenen la benditas cornetas?! y la respuesta a mi pregunta vino del [wiki de Gentoo](http://gentoo-wiki.com/Main_Page), ¿qué cosas no? imagínense todo lo que googleé; bueno específicamente de la sección que dice [VIA Envy24HT (ice1724) chip](http://gentoo-wiki.com/HOWTO_ALSA_sound_mixer_aka_dmix#VIA_Envy24HT_.28ice1724.29_chip). Ahora se preguntarán.. ¿VIA? éste tipo está loco.... pues sí, [VIA](http://www.via.com.tw/). ¿Y qué fué lo que hice? Abrí una terminal e hice lo siguiente:

	$ gedit .asoundrc

Luego lo que hice fué copiar el contenido del segundo .asoundrc y pegarlo al mío, luego guardar los cambios y voilá, sistema de sonido 5.1 andando ;). Se preguntarán _¿Cómo hizo éste loco para saber que ese .asoundrc funciona con la Audigy SE?_, bueno, con el proceso estándar, prueba error. Copiando y pegando cada uno de los ficheros.

Ahora que el sistema suena perfectamente bien me ha surgido una nueva interrogante, debido a que para poder graduar el volumen del sistema _tengo que graduar los tres controles_, y la pregunta es: ¿Habrá alguna forma de graduar el volumen para los tres controles al mismo tiempo? Que lo disfruten...
