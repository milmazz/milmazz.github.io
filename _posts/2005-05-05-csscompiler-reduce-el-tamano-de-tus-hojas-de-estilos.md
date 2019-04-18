---
title: CSScompiler, reduce el tamaño de tus hojas de estilos
author: milmazz
date: 2005-05-05 00:20:59
tags:
  - CSS
  - Recursos
slug: csscompiler-reduce-el-tamano-de-tus-hojas-de-estilos
redirect_from: /archivos/2005/05/05/csscompiler-reduce-el-tamano-de-tus-hojas-de-estilos/
---

[Daniel Mota](http://www.danielmota.com/) recientemente ha lanzado [CSScompiler
1.0](http://icebeat.bitacoras.com/archivo/121/csscompiler-10), se trata de un
_script_ que reduce al máximo el peso en _bytes_ (unidad básica de
almacenamiento de información) de tus hojas de estilo, esto puede ser
significativo si existe excesiva cantidad de peticiones a dichos ficheros, el
beneficio es **ahorrar ancho de bando en nuestros servidores**.

Ahora bien, ¿qué hace CSScompiler para reducir el tamaño de las hojas de estilos
en cascada?, simplemente elimina los comentarios, saltos de líneas y el último
punto y coma antes del cierre de los corchetes, además, se ofrecen otras
funcionalidades que mejoran la sintaxis e interpretación de algunas propiedades.

Puedes obtener una descripción más detallada en el artículo
[CSScompiler](http://icebeat.bitacoras.com/archivo/117/csscompiler). En el mismo
artículo podrás encontrar dos ejemplos (uno
[compilado](http://icebeat.bitacoras.com/csscompiler/css.php?css=css.css) y el
otro [sin compilar](http://icebeat.bitacoras.com/csscompiler/css.css)) que te
darán una idea acerca de la funcionalidad de este _script_.
