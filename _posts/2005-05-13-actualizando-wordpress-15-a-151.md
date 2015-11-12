---
redirect_from: "/archivos/2005/05/13/actualizando-wordpress-15-a-151/"
author: milmazz
comments: true
date: 2005-05-13 15:32:05
layout: post
slug: actualizando-wordpress-15-a-151
title: Actualizando WordPress 1.5 a 1.5.1
wordpress_id: 24
categories:
- WordPress
tags:
- Anuncios
- WordPress
---

Vía [Mundo Geek](http://www.mundogeek.net/) me entero del [anuncio](http://wordpress.org/development/2005/05/one-five-one/) de la nueva versión de [WordPress](http://www.wordpress.org), sistema manejador de contenidos que utiliza este sitio,  la cual es la 1.5.1, en esta nueva versión se ofrecen nuevas características importantes, arreglo de algunos errores, en cuanto a la seguridad se ha realizado un arreglo importante, por lo cual, es recomendable actualizar lo antes posible. Si desea ver un resúmen de las nuevas características, le recomiendo leer el [registro de cambios hechos a WordPress 1.5.1](http://codex.wordpress.org/Changelog/1.5.1). También se incluyen cambios en el tema clásico y el en que viene por defecto en la instalación 1.5 de WordPress.

Los pasos para actualizar desde la versión 1.5 a la versión 1.5.1 se describen en [Upgrade 1.5 to 1.5.1](http://wordpress.org/support/topic/33189), resumiendo un poco son los siguientes:

  1. Respalde el contenido de su base de datos.
  2. Descargue la versión 1.5.1 y descomprimala.
  3. Abra la carpeta wordpress, y borre la carpeta wp-images, esta carpeta no es necesaria para la actualización.
  4. Ahora, abra su cliente FTP (p.ej. [FileZilla](http://filezilla.sourceforge.net/)) y vaya al directorio de su bitácora.
  5. En el servidor, elimine los directorios wp-admin y wp-includes. **Nota:** En caso de tener el directorio languages (con ficheros con extensión .mo) dentro de la carpeta wp-includes, quizás ud. deba guardar/respaldar la carpeta languages antes de borrar el directorio wp-includes. Después de eliminar los directorios respectivos proceda a cargar los nuevos directorios wp-admin y wp-includes al servidor.
  6. El tema clásico y el que viene por defecto han sufrido pequeños cambios, entonces, si lo desea puede cargarlos dentro del directorio wp-content, tenga cuidado en no borrar el resto del contenido del directorio wp-content, manipule únicamente el contenido dentro del directorio themes.
  7. En la raíz del servidor donde se encuentra su bitácora, borre los viejos ficheros y cargue los nuevos. Es recomendable que haga esto archivo por archivo si no esta seguro de lo que está haciendo. No borre el fichero wp-config.php.
  8. Ahora escriba en la barra de direcciones de su navegador algo similar a: "www.example.com/wp-admin/upgrade.php", por supuesto, debe adaptar la dirección a sus necesidades.

Después de haber completado la actualización es conveniente que borre los siguientes ficheros dentro del directorio wp-admin.

  * install*.php
  * upgrade*.php
  * import*.php

**Actualización:** Al parecer existe un problema con la sindicación en esta nueva versión, para solucionarlo solamente se debe seguir los siguientes pasos.

  1. Identificarse, para ingresar en la opciones del panel administrativo.
  2. Opciones -> Lectura -> Presiona el botón **Actualizar opciones**.
  3. Abra cualquier entrada y presione el botón **Guardar**
