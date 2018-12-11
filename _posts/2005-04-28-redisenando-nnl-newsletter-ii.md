---
author: milmazz
comments: true
date: 2005-04-28 06:30:31
layout: post
slug: redisenando-nnl-newsletter-ii
title: Rediseñando NNL Newsletter II
tags:
- CSS
---

En este artículo se mostrará la facilidad de emplear hojas de estilos en cascada o CSS cuando poseemos una buena estructura en nuestros documentos. Como estructura vamos a utilizar una [modificación](http://blog.milmazz.com.ve/wp-content/nnl/estructura.html) que he realizado de la [primera edición de NNL Newsletter](http://nnlnews.com/nnl/1). La edición de la estructura se ha explicado en el artículo [Rediseñando NNL Newsletter I](/article/2005/04/28/redisenando-nnl-newsletter-i/).

<!-- more -->

### Estableciendo valores a los márgenes y rellenos

Usualmente mi primera regla en CSS es establecer los márgenes y rellenos de **todos** los elementos XHTML (o HTML) a **cero** (0), ¿por qué hacer esto?, simplemente porque los Agentes de Usuario (p.ej. Navegadores) implementan distintas reglas sobre estas dos propiedades. Desde mi punto de vista, la mejor forma de controlar estas diferencias es ir estableciendo los valores de dichas propiedades de acuerdo a las necesidades que tengamos, aunque previamente se han inicializado a **cero**.

Muestra de ejemplo en CSS

    <code>*{
      margin: 0;
      padding: 0;
    }</code>

### Creando nuestro Layout

Antes que nada debemos pensar en que tipo de contenido mostraremos y que extensos serán estos. En nuestro ejemplo de análisis, NNL Newsletter se basa _principalmente_ en texto, estos textos suelen ser extensos, por lo que debemos ser precavidos a la hora de mostrarlos, la lectura no debe "cansar" al lector, debemos mostrar el mayor contraste posible. Partiendo de las características del sitio en particular, podemos concluir que lo mejor es implantar un _layout_ _elástico_, este tipo de diagramación simplemente se basa en el concepto de medidas relativas tanto en los bloques de la página como en las tipografías, por lo que al usuario se le facilitará la ampliación (o disminución) de los elementos mostrados desde el panel de control del navegador.

Inicialmente vamos a centrar la página.

Muestra de ejemplo en CSS

    <code>body{
      text-align: center;
    }
    #wrapper{
      margin: 3em auto;
      width: 35em;
      text-align: left;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/01.html).

En las dos reglas anteriores, quien realmente hace el trabajo de centrado del documento es la declaración `margin: 3em auto;`, en donde se especifica que tanto el margen superior como el margen inferior sean iguales a `3em`, los márgenes izquierdo y derecho tomarán valores automáticos idénticos, lo cual centrará nuestro documento, ahora bien, para que la declaración anterior funcione correctamente debe especificarse la anchura del bloque que queremos centrar, en nuestro ejemplo hemos seleccionado el valor de `35em`. Respecto a la utilización de la declaración `text-align` sobre el selector `body` simplemente es para ampliar la compatibilidad de nuestro diseño con los usuarios de IE5 en sistemas Windows, sin dicha declaración la mayoría de los navegadores aún muostrarían el layout centrado, pero IE5/Win no.

Respecto al valor que se le ha asignado a la propiedad `width` que aplicara sobre el bloque `#wrapper`, la explicación dada por [Nicolás Fantino](http://www.100px.com) en el artículo [Ni fijo, ni líquido. Elástico](http://www.100px.com/articulos/ni_fijo_ni_liquido_elastico/), seguramente aclarará las posibles dudas.

> Cualquiera que se haya dedicado en algún momento de su vida al diseño gráfico editorial sabrá que existen, o al menos habrá escuchado hablar de, ciertas normas o convenciones. Una de ellas define lo que se considera el ancho óptimo de una línea de texto para ser leído en bloque. Éste es de entre 30 em y 35 em. La unidad de medida de este ancho es el "em". Un "em" mide exactamente el ancho de la letra "M" mayúscula de una tipografía dada y a un tamaño dado. Efectivamente, según esta definición un "em" no mide físicamente siempre lo mismo. ¿Por qué se usa esta medida? porque el ancho óptimo para la lectura dependerá, necesariamente, del tipo de letra que se use y, más necesariamente aún, del tamaño de ésta.

Para ampliar la compatibilidad del _layout_ _elástico_ con el navegador IE (sí, otra vez), debemos previamente definir un valor cuya unidad de medida sea el **porcentaje** a la tipografía en el selector `body`, por ejemplo:

    <code>body{
      font-size: 85%;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/02.html).

Posteriormente dejaremos de usar la unidad de medida **porcentaje**, de ahora en adelante utilizaremos como unidad de medida `em`.

Antes de continuar, vamos a "maquillar" un poco nuestro _layout_.

    <code>body{
      font: 85%/145% "Trebuchet MS", Arial, Verdana, sans-serif;
      color: #333;
      border-top: 5px solid #bbb;
      background: #eee url(bg-bottom.png) repeat-x bottom left fixed;
    }
    #wrapper{
      border: 1px solid #bbb;
      border-top: 5px solid #bbb;
      background: #fff;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/03.html).

Solamente hemos "jugado" un poco con los valores de: fuentes, colores, bordes y fondos. Se han utilizado modos abreviados de escritura de código CSS para ahorrarnos unos cuantos bytes.

### Sustituyendo texto por una imagen

Desde mi punto de vista, es preferible no hacer uso de imágenes como título más importante, es conveniente utilizar un encabezado `h1`, pero a veces resulta conveniente sustituir ese título por el logo del sitio en particular, existen muchos métodos para hacerlo a través de CSS, yo utilizaré el método **Shea Enhancement** planteado por [Dave Shea](http://www.mezzoblue.com/) al final del artículo [Revised Image Replacement](http://www.mezzoblue.com/tests/revised-image-replacement/)

Nos aprovecharemos de la siguiente estructura en XHTML.

    <code><h1 id="header" title="NNL Newsletter"><a href="http://www.nnlnews.com/"><span></span>NNL Newsletter</a></h1></code>

La imagen que sustituirá al título tiene una anchura de `134px` y una altura de `130px`. Ahora recurrimos a CSS y hacemos lo siguiente:

    <code>h1{
    font-size: 1.2em;
    }
    #header{
      width: 134px;
      height: 130px;
      position: relative;
    }
    #header span{
      background: url(logo.png) no-repeat top left;
      width: 100%;
      height: 100%;
      position: absolute;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/04.html).

### Notas

Carlos Tori, encargado de la redacción de NNL Newsletter, acostumbra colocar un párrafo de notas al principio de las distintas ediciones, para distinguir dicho párrafo, hemos creado un clase llamada `note`, funciona de la siguiente manera:

    <code>p{
      margin: 0 1em 0.5em 1em;
    }
    p.note{
      border-top: 2px solid #666;
      background: #f5f5f5;
      padding: 1em;
      margin: 1em 0;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/05.html).

En primera instancia declaramos los márgenes necesarios a todos los párrafos (`p`), luego vamos al caso particular de la nota, la cual, consiste de una párrafo cuya clase sea igual a `note`.

### Listas Ordenadas

Carlos Tori, siempre coloca una lista ordenada de los puntos que tratará en la edición de NNL Newsletter, es conveniente implantar una lista ordenada de enlaces, esto facilitará el acceso a los diversos puntos que se tratan.

    <code>ol{
      margin: 1em 3em;
    }
    ol li{
      list-style-type: none;
      background: url(bullet.png) no-repeat left center;
      padding-left: 16px;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/06.html).

La primera regla aplica sobre las listas ordenadas, en ella establecemos los márgenes. En la segunda regla, en primera instancia evitamos mostrar el marcador de los ítems de la lista, esto lo hacemos a través de la propiedad `list-style-type` con un valor igual a `none`, luego empleamos la propiedad `background` para colocar un marcador de los ítems partiendo de una imagen, la cual estará posicionada hacia la izquierda horizontalmente y estará posicionada verticalmente en la parte central, esto se logra con las palabras claves `left` y `center` respectivamente, antes de finalizar la segunda regla, controlamos el relleno izquierdo con un valor fijo.

### Listas de definición

De acuerdo a la estructura de NNL Newsletter, normalmente se plantea un tópico e inmediatamente se procede a describirlos, como Carlos define ciertos tópicos, me parece adecuado agruparlos dentro de una lista de definiciones. Ahora bien, vamos a "maquillar" dichas listas a través de las hojas de estilos en cascada.

Primero, vamos a encargarnos de los títulos de las definiciones.

    <code>dt{
      font-weight: bold;
      font-size: 1.1em;
      background: #eee;
      margin-top: 14px;
      padding: 6px 6px 7px 12px;
      border-top: 2px solid #bbb;
    }</code>

En la regla anterior, vamos a darle cierto **peso** (`font-weight`) a los títulos, definimos el tamaño de la fuente (`font-size`), seleccionamos un color de fondo (`background`), establecemos un margen superior (`margin-top`), establecemos los rellenos (`padding`) y finalmente decoramos los títulos con un borde superior (`border-top`).

Ahora definamos los rellenos de las descripciones de las definiciones.

    <code>dd{
      padding: 6px 6px 10px 8px;
    }</code>

[Vea el resultado de aplicar la regla de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/07.html).

Si detalla la [muestra anterior](http://blog.milmazz.com.ve/wp-content/nnl/07.html), puede darse cuenta que después del tópico cuyo título de definición es: _Feedback: Los lectores preguntaron..._, la descripción de la definición de dicho es tópico es otra lista de definiciones, es decir, existe una lista de definiciones anidada, recuerde que las listas de definiciones también pueden utilizarse para representar dialogos según la especificación del XHTML. Vamos a diferenciar dicha lista de definiciones del resto.

    <code>#feedback dl{
      background: #ffe;
      border: 1px solid #999;
      border-top: 0;
      margin: 13px 2px 8px 2px;
      padding: 0 5px 10px 5px;
      color: #000;
    }
    #feedback dt{
      font-size: 0.95em;
      color: #fff;
      border: 0 none;
      background: #c30;
      margin: 0 -5px;
      padding: 4px 10px;
    }
    #feedback .nnl, #feedback .ans{
      background: #fcfcfc;
      margin: 0 5px;
      border-right: 1px solid #bbb;
      border-left: 1px solid #bbb;
    }
    #feedback .ans{
      color: #333;
      border-top: 1px solid #bbb;
      padding: 5px 11px 5px 9px;
    }
    #feedback .nnl{
      border-bottom: 1px solid #bbb;
      padding: 0 11px 10px 9px;
    }
    #feedback .to, #feedback .subject, #feedback .date{
      font-style: italic;
    }</code>

[Vea el resultado de aplicar las reglas de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/08.html).

Simplemente es un "juego" con las propiedades: `background`, `border`, `margin`, `padding`, `font-size`, `color` y `font-style`; las cuales han sido explicadas con anterioridad.

Quizás deba resaltar la propiedad `margin` utilizada en la regla `#feedback dt`, la cual tiene valores negativos (`-5px`) para los márgenes izquierdo y derecho, esto lo hago simplemente con el fin de ocupar todo el espacio de la lista de definición, en la cual se ha definido previamente un relleno (`padding`) de `5px`.

### Código

Vamos a mejorar la presentación del código.

    <code>code{
      font-family: "Courier New", Courier, monospace;
      background: #ffe;
      font-size: 0.95em;
    }
    code.block{
      overflow: auto;
      width: 33em;
      display: block;
    }
    pre{
      text-align: center;
      background: #ffe;
      color: #000;
      width: 33em;
      border: 1px solid #bbb;
      margin: 6px auto;
      padding: 0;
      overflow: auto;
      height: 100%;
    }
    pre code{
      display: block;
      text-align: left;
      margin: 6px 7px;
      background: transparent;
    }</code>

[Vea el resultado de aplicar las reglas de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/09.html).

En primer lugar, se le asigna una tipografía distinta de la normal, para distinguir el código del resto del flujo, en algunas ocasiones Carlos emplea en línea código extremadamente largo, lo cual causa que el código rompa con el _layout_, por ello he creado una clase llamada `block` que convertirá el selector `code`, el cual es un elemento en línea, en un elemento en bloque. Para el caso de los bloques de código, estos se han centrado, para lograr tal efecto he utilizado el mismo método citado en principio para centrar el _layout_.

Es importante resaltar, que el único concepto aplicado que puede resultarle nuevo es el uso de la propiedad `overflow`, la cual solamente puede ser aplicada a elementos en bloque, esta propiedad es utilizable cuando el contenido sobresale o desborda de la caja que lo contiene, cuando esto sucede, se proporcionan barras de desplazamiento para visualizar el resto del contenido, evitando así romper el _layout_.

### Enlaces

Es importante resaltar que ciertos navegadores podrían ignorar una o más reglas de _pseudo clases_ en los enlaces, a menos que dichas _pseudo clases_ sean listadas en el orden siguiente: `:link`, `:visited`, `:hover` y `:active` (LVHA). Según el Sr. [Zeldman](http://www.zeldman.com/) existe un mnemónico anglosajón que podría ayudarle a recordar dicho orden, dicho mnemónico es LoVe-HA!.

Seguramente ud. se estará preguntando en este instante lo siguiente: ¿qué es un selector de pseudo-clase?, en el mundo de las hojas de estilos en cascada, una clase es aquella que es especificada explicitamente con el atributo `class` dentro de la estructura del documento XHTML. Una _pseudo clase_ es aquella que depende de la actividad del usuario o del estado que indique el navegador (`:hover`, `:visited`).

    <code>a:link, a:visited{
      font-size: 0.85em;
      color: #c30;
      background: transparent;
      font-weight: bold;
      text-decoration: none;
    }
    a:hover, a:active{
      color: #999;
      background: transparent;
      text-decoration: underline;
    }
    a[hreflang]:after{
      content: " [" attr(hreflang) "]";
    }
    #footer a:hover {
      color: #666;
      text-decoration: underline;
    }</code>

[Vea el resultado de aplicar las reglas de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/10.html).

Posiblemente la regla más complicada de las mostradas anteriormente es `a[hreflang]:after`, voy a explicar brevemente de que trata todo esto, `a[hreflang]`, simplemente aplicará sobre **todos** aquellos enlaces que posean el atributo `hreflang` sin importar el valor que tengan. La propiedad `content` es utilizada en conjunto con los _pseudo elementos_ `:before` o `:after` para generar contenido antes o después del elemento respectivamente, dentro de la propiedad `content` se utilizan dos cadenas de carácteres, tanto el corchete que abre como el corchete que cierra, dentro de los corchetes se utiliza el valor `attr(hreflang)`, el cual devolverá como una cadena el valor del atributo `hreflang` ubicado dentro del selector `a`.

### Pie de página

El pie de página normalmente es utilizado para indicar el tipo de _copyright_ del contenido o algún tipo de firma en particular.

Se han realizado unos ajustes al pie de página, estos son los siguientes:

    <code>#footer{
      background: #fff url(bg-footer.png) repeat-x top left;
      color: #333;
      border-top: 2px solid #666;
    }
    #footer p{
      margin: 0;
      padding: 4px 9px 3px 10px;
    }
    #footer a{
      color: #000;
    }</code>

[Vea el resultado de aplicar las reglas de estilo a la estructura del documento](http://blog.milmazz.com.ve/wp-content/nnl/11.html).

Todas las propiedades utilizadas en estas reglas se han descrito previamente, no considero necesario volver a hacerlo.

#### Referencias

  * [ Ni fijo, ni líquido. Elástico](http://www.100px.com/articulos/ni_fijo_ni_liquido_elastico/)

  * [CSS Centering 101](http://www.simplebits.com/notebook/2004/09/08/centering.html)

  * [Revised Image Replacement](http://www.mezzoblue.com/tests/revised-image-replacement/)

  * [CSS in Action: A Hybrid Layout (Part II)](http://www.zeldman.com/dwws/pdfs/0735712018C_10.pdf)

  * [Fundación Sidar](http://www.sidar.org/)

  *[ud.]: usted
  *[Sr.]: Señor
  *[p.ej.]: por ejemplo
