---
author: milmazz
comments: true
date: 2005-04-14 18:34:15
layout: post
slug: galeria-de-imagenes
title: Galería de Imágenes
wordpress_id: 4
tags:
- (X)HTML
- CSS
---

En este artículo se describirá un método para implantar una galería de imágenes, esta guía será básica, se describirá la estructura (haciendo uso de XHTML) del documento, adicionalmente nos encargaremos de la presentación de la galería, haciendo uso de hojas de estilos en cascada o CSS. El tema del comportamiento de la galería lo dejaremos a criterio del usuario, ya que existen diversas formas para implantar este sistema, algunas más convenientes que otras.


<!-- more -->


En primera instancia, vamos a realizar la estructura del documento



    
    <code><body>
    <div id="wrapper">
       <div id="main">
          <p><img src="587x474.gif" alt="Texto Alternativo"
          width="587px" height="474px" /></p>
       </div>
       <div id="thumbs">
          <h2 id="muestras" title="Imágenes de Muestra">
          <span></span>Imágenes de muestra</h2>
          <ul>
              <li><a href="#">
              <img src="75x75.gif" alt="Texto Alternativo"
              width="75px" height="75px" /></a></li>
              ...
          </ul>
       </div>
    </div>
    </body></code>




Vamos a explicar poco a poco la estructura del documento, en primer lugar se ha creado una división, `wrapper`, se utilizará para envolver todo el contenido, adicionalmente nos permitirá centrar la página (presentación) a través de hojas de estilo en cascada, a continuación se ha anidado otra divisón, la division  `main`, en ésta será donde se expondrá la imagen principal, es decir, aca se expondrá la imagen más reciente o en caso que el usuario seleccione un _thumbnail_ (los cuales se ubican dentro de la división `thumbs`), se mostrará una imagen con mayores dimensiones en la división `main`. Como se menciono anteriormente este comportamiento no será descrito, lo dejamos a criterio del usuario.




Cabe resaltar que en la división `thumbs`, se implementará en la etiqueta `h2` un reemplazo de texto por una imagen, he seguido el método **Shea Enhancement** expuesto por [Dave Shea](http://www.mezzoblue.com), el cual es explicado al final del artículo [Revised Image Replacement](http://www.mezzoblue.com/tests/revised-image-replacement/), también puede ampliar esta noticia en la excelente recopilación hecha por [Kemie Guaida](http://www.monolinea.com/) en el artículo Reemplazo de Textos - una comparación, allí podrá evaluar las distintas opciones existentes, sus ventajas y desventajas.




Ahora bien, comencemos con la presentación del documento. En primer lugar, debemos tener claro que los distintos navegadores presentan de manera distinta ciertos márgenes (`margin`) y rellenos (`padding`) de manera predeterminada, para evitar complicarnos la vida, es recomendable comenzar nuestra hojas de estilos en cascada anulando dichos márgenes y rellenos para todos los elementos XHTML, de la siguiente manera:



    
    <code>* { margin: 0; padding: 0; }</code>




Con la regla anterior simplemente estamos **obligando** a todos los elementos tener márgenes y rellenos iguales a 0 (**cero**). Ahora bien, vamos a crear una regla para el cuerpo del documento (`body`).



    
    <code>body{
       font-size: small;
       font-family: Georgia, "Times New Roman", serif;
       background: #369 url(bg_bottom.gif) bottom left fixed repeat-x;
    }</code>




Brevemente se explicará esta regla, aunque la mayoría de las declaraciones se explican por sí solas, esta regla se aplicará al **selector** `body`, dicho selector tendrá un tamaño en la fuente pequeño, la precedencia de las fuentes viene dada de izquierda a derecha, por ejemplo, la fuente de preferencia es **Georgia**, en caso de fallo, se utilizará la fuente **Times New Roman**, en caso de fallo, se utilizará la fuente predeteminada de la familia **serif**. Es importante recalcar que aquellos nombres de fuentes que poseen carácteres de espacio, debemos encerrarlas entre comillas dobles. La última declaración se encarga del fondo, se ha establecido el color de fondo al código hexadecimal `#336699` (se ha utilizado una abreviatura que permiten las hojas de estilos en cascada), adicionalmente, se ha colocado una imagen en la parte inferior (`bottom`) izquierda (`left`) del cuerpo del documento, dicha imagen se repetirá horizontalmente (`repeat-x`), el valor `fixed` simplemente nos permitirá mantener fija la imagen aún cuando se realicen  desplazamientos.




A continuación se procederá a centrar el documento, para ello nos referiremos a la divisón cuya `id` es `wrapper`, adicionalmente se agregará una declaración al selector `body` para evitar inconvenientes en la mala interpretación que hace IE.



    
    <code>body{ text-align: center; }
    #wrapper{
       width: 603px;
       margin: 20px auto;
       text-align: left;
       background: #fff;
       border: 1px solid #333;
    }</code>




Para no extenderme demasiado (y evitar así que ud. se duerma) en la explicación de esta guía básica, le recomiendo lea el artículo [CSS Centering 101](http://www.simplebits.com/notebook/2004/09/08/centering.html) de [Dan Cederholm](http://www.simplebits.com/).




El método aplicado anteriormente para centrar el _layout_ entero, también puede aplicarse a otros elementos en bloque. Por ello, vamos a aplicar el mismo método a la imagen principal, la ubicada en la división `main`, pero debe recordar que una imagen (`img`) es un elemento en línea, así que vamos a "convertirlo" en un elemento en bloque por medio de CSS.



    
    <code>#main img{
       display: block;
       margin: 0 auto;
    }</code>




Ya para finalizar, vamos a concentrarnos en la lista desordenada de imágenes que se encuentran anidadas en la división`thumbs`. Sabemos que los elementos que se presentan en las listas son elementos en bloque, si queremos presentar uno al lado del otro debemos en primera instancia convertirlos en elementos en línea (esto lo haremos a través de la propiedad `display`), adicionalmente, debemos eliminar la apariencia de los marcadores de los ítems de la lista, para lograr esto, nos referiremos a la propiedad `list-style-type`, finalmente, controlaremos los márgenes.



    
    <code>#thumbs li{
       list-style-type: none;
       display: inline;
       margin: 10px -4px 0 8px;
    }</code>




Es recomendable que examine el código fuente de la [Galería de Imágenes](http://blog.milmazz.com.ve/wp-content/gallery.html) que se ha creado, fijese que se han agregado algunas líneas de codigo CSS, pero la base se ha explicado en esta guía.
  *[ud.]: usted
  *[IE]: Internet Explorer
