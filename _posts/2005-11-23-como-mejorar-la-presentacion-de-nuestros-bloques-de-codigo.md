---
author: milmazz
comments: true
date: 2005-11-23 13:45:00
layout: post
slug: como-mejorar-la-presentacion-de-nuestros-bloques-de-codigo
title: Cómo mejorar la presentación de nuestros bloques de código
tags:
- (X)HTML
- CSS
---

El día de ayer en la entrada [Ctrl + Alt + Supr en
ubuntu](/article/2005/11/21/ctrl-alt-supr-en-ubuntu/) el amigo (gracias al
canal `ubuntu-es` en el servidor Freenode) [_Red_](http://red.psykho.net/) me
preguntaba lo siguiente:

> quisiera saber que plugin de WP usas para que salga en un recuadro los comandos de la terminal?

Ahora bien, como en principio la pregunta no tiene que ver con la temática de
la entrada, además, mi respuesta puede que sea algo extensa, prefiero
responderle a través de esta entrada.

En realidad para la presentación de los bloques de código no hago uso de ningún
agregado, solo uso **correctamente** (sin animos de parecer ostentoso) las
etiquetas que nos ofrece el lenguaje de marcado XHTML (_lenguaje extensible de
marcado de hipertexto_), dándole semántica a la estructura de la entrada, la
presentación de dicho bloque de código lo realizo a través del uso de CSS
(_hojas de estilo en cascada_).

En primer lugar vamos a ver como **debe** ser la estructura de los bloques de
código.

    <code><pre><code>#include <iostream>

    int main()
    {
    std::cout << "Hola Mundo!!!" << std::endl;
    return 0;
    }
    </code></pre></code>

Vea el [ejemplo #1](http://blog.milmazz.com.ve/wp-content/ejemplos.de.bloques.de.codigo/1.html).

El bloque de código anterior muestra un programa muy sencillo en `C++`.

### Estructura XHTML

Es hora de definir algunos conceptos muy interesantes.

#### El elemento `<pre>`

En primer lugar debemos recordar que el elemento <pre> es un _elemento en
bloque_, los agentes de usuario _visuales_ entenderán que el texto contenido
dentro de este elemento vendrá con un formato previo.

Lo anterior implica ciertas ventajas, por ejemplo, pueden dejarse espacios en
blanco, los cuales serán interpretados tal cual como se manifiestan de manera
explícita. Adicionalmente, se pueden representar fuentes de ancho fijo dentro
de estos bloques.

#### El elemento `<code>`

El elemento <code> es un elemento en línea, la semántica detrás de este
elemento es indicar segmentos de código.

### Mejorando la presentación del bloque de código

Una vez que comprendamos la estructura que **deben** seguir nuestros bloques de
código, debemos hacer uso de las _hojas de estilos en cascada_ para la
presentación de dichos bloques. Esto será necesario realizarlo una sola vez.

Mi gusto particular es centrar los bloques de código, esto no tiene porque ser
entonces una regla estándar, a continuación describiré como realizar esto vía
CSS, solamente debemos seguir las siguientes reglas.

    <code>pre{
      text-align: center;
      width: 30em;
      margin: 1em auto;
      white-space: pre; /* CSS 2 */
    }
    pre code{
      display: block;
      text-align: left;
    }</code>

Vea el [ejemplo #2](http://blog.milmazz.com.ve/wp-content/ejemplos.de.bloques.de.codigo/2.html).

Al selector `pre` le he asignado una anchura de `30em`, este valor es relativo
a la fuente, pero también podría especificarse en `px`, es importante resaltar
que haciendo uso de la unidad `em` se permite generar un bloque líquido.

La declaración que está realizando _todo_ el trabajo de centrar el bloque es
`margin: 1em **auto**;`, en ella se indica que tanto el margen superior como
inferior sea de `1em`, de igual manera se especifica que tanto el margen
izquierdo como el derecho sean `auto`, por lo tanto, sus valores serán iguales,
esto nos asegura que la _caja_ quede centrada.

Ahora bien, para brindar mayor accesibilidad a nuestro bloque de código, será
necesario hacer uso de la propiedad `text-align: center;` en el selector `pre`,
la razón por la cual se usa la declaración anterior es para brindar una buena
presentación en aquellos usuarios de IE5 bajo sistemas Windows. Sin esta regla,
la mayoría de los agentes de usuario visuales podrán mostrar el bloque de
código centrado, pero no aquellos usuarios de IE5/Win.

La declaración `white-space: pre;` se utiliza para especificar como serán
tratados los _espacios en blanco_ dentro del elemento. Cuando se indica el
valor `pre` los agentes de usuario visuales impedirán el cierre de las
secuencias de espacios en blanco.

Finalmente, en el selector `pre code`, debemos reescribir la declaración de
alineación del texto (`text-align`). En ella estamos alineando el texto de
nuevo a la izquierda, si no lo hacemos, el texto se mostrará centrado debido a
la declaracion de alineación de texto en el selector `pre`.

El uso de la declaración `display: block;` modifica la manera en que se muestra
el elemento `code`, como se mencionó previamente, el elemento `code` es un
_elemento en línea_, al hacer uso de esta instrucción, nos permitirá mostrar al
elemento `code` como un _elemento en bloque_.

Ahora bien, todavía existe una interrogante que debemos contestar, dicha
interrogante es: _¿Qué sucede si el contenido del bloque de código es demasiado
extenso horizontalmente?_, simplemente el texto se desbordará por encima del
bloque, esto es un problema, pero existen dos maneras de solucionarlo.

#### ¿Cómo solucionar un posible desborde del contenido sobre el bloque de código?

La primera solución que podriamos pensar es hacer uso de la declaración
`overflow: auto;`, la propiedad `overflow` especifica si el contenido de un
_elemento en bloque_ puede ser recortado o nó cuando éste desborda a dicho
elemento. El valor `auto` nos permite proporcionar un mecanismo de
desplazamiento en el caso de aquellas cajas que presenten un desborde.

La solución anterior también implica otra inquietud, en este caso particular,
la **usabilidad**, según el gurú de la _usabilidad_, [Jakob
Nielsen](http://www.useit.com/), los usuarios detestan el tener que hacer uso
de las barras de desplazamiento horizontales, el parecer el desplazamiento
vertical parece estar bien, puesto que es más común.

Por los motivos descritos en el párrafo anterior, el hacer uso de la barra de
desplazamiento horizontal no es la mejor solución. Veamos la solucion
**definitiva**.

La única posibilidad que tenemos para evitar hacer uso de la barra de
desplazamiento horizontal es que al ocurrir un desborde, el contenido que
desborda pase a la siguiente línea.

Ahora bien, lo que se mostrará a continuación puede resultarle confuso, no se
preocupe, trataré de explicarlo, pero recuerde, no soy ningún experto, solo un
entusiasta :)

    <code>pre{
      /* Reglas especificas para algunos navegadores y CSS 3 */
      white-space: -moz-pre-wrap; /* Mozilla, soportado desde 1999 */
      white-space: -hp-pre-wrap; /* Impresoras HP */
      white-space: -o-pre-wrap; /* Opera 7 */
      white-space: -pre-wrap; /* Opera 4-6 */
      white-space: pre-wrap; /* CSS 2.1 */
      white-space: pre-line; /* CSS 3 (y 2.1 tambien, actualmente) */
      word-wrap: break-word; /* IE 5.5+ */</code>

Vea el [archivo maestro](http://blog.milmazz.com.ve/wp-content/ejemplos.de.bloques.de.codigo/master.html).

La versión original del código mostrado previamente pertenece a [Ian
Hickson](http://ln.hixie.ch/) quien distribuye su trabajo bajo licencia
[GPL](http://www.gnu.org/copyleft/gpl.html).

Bajo [CSS2](http://w3.org/TR/CSS2/), no existe una manera explícita de indicar
que los espacios y nuevas líneas deban preservarse, pero en el caso en el cual
el texto alcance el extremo del bloque que le contiene, se le puede envolver.
Lo más cercano que nos podemos encontrar es `white-space: pre`, sino no es
posible envolverlo. Antes de que [CSS2.1](http://w3.org/TR/CSS21/) sea la
_recomendación candidata_, los agentes de usuario no podrán implementarla, por
eso se han implementado ciertas extensiones propietarias, en el código mostrado
previamente se muestran todas estas posibilidades, los agentes de usuario
tomarán aquellas declaraciones que soporten.

La declaración `word-wrap: break-word` es una extensión propietaria de IE, la
cual no es parte de ningún estándar.

Para dejar las cosas en claro, `pre-wrap` actúa como pre pero cubrirá cuando
sea necesario, mientras que `pre-line` actúa como `normal` pero preserva nuevas
líneas. Según la opinión de [Lim Chee
Aun](http://cheeaun.phoenity.com/weblog/), la propiedad `pre-wrap` será
realmente útil en aquellos casos en los cuales deban mostrarse largas líneas de
código que posiblemente desborden en otros elementos o simplemente se muestren
fuera de pantalla.

Ahora bien, ¿qué hay del soporte de los agentes de usuario visuales?, bien, la
mayoría de los navegadores modernos soportan correctamente la propiedad `pre`,
`normal` y `nowrap`. Firefox soporta la propiedad `-moz-pre-wrap` pero no
soporta `pre-wrap` y `pre-line` todavía. Opera 8 soporta `pre-wrap` incluyendo
sus extensiones previas, `-pre-wrap` y `-o-pre-wrap`, pero no `pre-line`.

Referencias:

  * [Top Ten Web-Design Mistakes of 2002](http://www.useit.com/alertbox/20021223.html)
  * [Scrolling and Scrollbars](http://www.useit.com/alertbox/20050711.html)
  * [CSS Centering 101](http://www.simplebits.com/notebook/2004/09/08/centering.html)
  * [Whitespace and generated content](http://cheeaun.phoenity.com/weblog/2005/06/whitespace-and-generated-content.html)

[IE]: Internet Explorer
