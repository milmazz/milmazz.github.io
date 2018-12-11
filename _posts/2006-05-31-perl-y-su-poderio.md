---
redirect_from: "/archivos/2006/05/31/perl-y-su-poderio/"
author: milmazz
comments: true
date: 2006-05-31 03:47:15
layout: post
slug: perl-y-su-poderio
title: Perl y su poderío
categories:
- perl
tags:
- linux
- perl
- velug
---

Al igual que [José](http://blog.bureado.com.ve/), considero que el hilo de discusión [Borrar línea X de un archivo](http://www.velug.org.ve/pipermail/l-linux/2006-May/059328.html) es **bastante** interesante, este hilo fué discutido en la lista de correos técnica [l-linux](http://velug.org.ve/cgi-bin/mailman/listinfo/l-linux) es la la lista de correos para Consultas Técnicas sobre Linux y Software Libre en VELUG del [Grupo de Usuarios de Linux de Venezuela](http://velug.org.ve/) (VELUG), el problema planteado por quien inicio el hilo de discusión, José Luis Bazo Villasante, consistía en eliminar un _registro_ completo, en donde se pasara como argumento el primer campo (tal vez el _identificador_) de dicho registro.

Suponga que el fichero tiene la siguiente estructura:

    :(123
    	... # otros campos
    )

    :(234
    	... # otros campos
    )

    :(456
    	... # otros campos
    )

Se dieron soluciones en lenguajes como `Bash` y `C` ¿Con intención de autoflagelación? y ciertas en [Perl](http://perl.com/), éstas últimas son las que llaman mi atención, _vamos por partes_ Diría Jack El Destripador.

José propuso lo siguiente:

{% highlight perl %}
#!/usr/bin/perl -n
$deadCount = 7 if ($_ =~ /${ARGV[0]}/);
--$deadCount if ($deadCount);
print unless ($deadCount);
{% endhighlight %}

El programa debe ejecutarse así:

	$ perl script.pl archivo 123

Este programa hace el trabajo, pero al final emitirá un error porque cree que el argumento 123 es otro fichero, y por supuesto, no lo encuenta.

Mi solución fue la siguiente:

{% highlight perl %}
perl -ne 'print unless /:\\(123/../\\)/' input.data > output.data
{% endhighlight %}

Por supuesto, en este caso solamente estaría eliminando el registro cuyo primer elemento es 123, **funciona**, pero genera un problema al igual que el hecho por José, se deja una línea de espacio vacía adicional en los registros, cuando el separador de los grupos de datos debe ser **una** No dos, ni tres, ... línea en blanco, tal cual como apunto Ernesto Hernández en una de sus respuestas.

Otro apunte realizado por el profesor Ernesto, fué que las soluciones presentadas hasta el momento de su intervención fué el _análisis_ de fondo que estabamos haciendo, el problema no consistía en procesar cada una de las líneas, el trabajo en realidad consistía en analizar un registro multilínea (o párrafo), en términos más sencillos, cada grupo de datos (registros) está separado por una línea en blanco.

El profesor continuaba su excelente explicación diciendo que el problema se reduce al analizarlo de esta manera en lo siguiente:

> ...si se cuenta con un lenguaje de programación que está preparado para manejar el concepto de "registro" y puede definir el separador de registro como una línea en blanco, simplemente se trata de ignorar aquellos registros que tengan la expresión regular (X, donde X es la secuencia de dígitos que **no** nos interesa preservar.

La solución presentada por el profesor Ernesto fue:

{% highlight perl %}
perl -000 -i -ne 'print unless /\\(XXX/' archivo
{% endhighlight %}

En donde se debe sustituir las XXX por los dígitos cuyo bloque no nos interesa conservar.

De este hilo aprendí cosas nuevas de Perl, en realidad estoy comenzando, muchos pensarán que este código es críptico, por ello considero conveniente aclarar algunas cosas.

Lo críptico de un código **no** es inherente a un lenguaje particular, eso depende más del _cómo_ se programe.

En este caso particular una sola línea de código nos proporciona mucha información, evidentemente para comprender dicho contenido es necesario leer previamente cierta documentación del lenguaje, pero ¿quien comienza a programar en un lenguaje en particular sin haber leído primero la documentación?, la respuesta parece lógica, ¿cierto?.

La existencia de opciones predefinidas y maneras de ejecutar el interprete de Perl permiten enfocarse únicamente en la resolucion de tareas, _cero burocracia_.

Un ejemplo de lo mencionado en el párrafo anterior es el siguiente, un bucle lo puedo reducir con la opción de ejecución `-n` del interprete Perl, simplemente leyendo un poco perlrun`man perlrun`, perlrun se incluye en la documentación de Perl, en sistemas Debian lo encontramos en el paquete perl-doc, para instalar simplemente hacer ejecutar el comando `aptitude install perl-doc` como _superusuario_ nos enteramos del asunto, eso quiere decir que podemos reducir a una simple opción de ejecución del interprete de Perl todo esto:

{% highlight perl %}
#!/usr/bin/perl
while(<>){
#...
}
{% endhighlight %}

¿Qué es ese operador que parece un "platillo volador", según nos conto José Luis Rey, el profesor Ernesto Hernández le llama así de manera informal (_null filehandle_)?, bueno, lea perlop, en especial la sección I/O Operators.

La opción -i me permite editar (_reescribir_) _in situ_ el fichero que vamos a procesar, en el caso de no añadir una extensión no se realizara un respaldo del fichero, el fichero original se sobreescribe. Mayor detalle en perlrun.

Lo que si no sabía hasta ahora, es lo explicado por el profesor acerca de los _párrafos_ (registros multilínea) en Unix, la opción -0 tiene un valor especial, el cual es 00, si este valor es suministrado hace que Perl entre en "modo párrafo", en pocas palabras, se reconoce una línea en blanco como el separador de los registros.

El resto del código es solo manejo de una sencilla expresión regular, se asume que el lector conoce algo del tema, solo indica el registro que queremos **ignorar**.

Así que podemos concluir lo siguiente, _Perl no es críptico_, asumiendo que el programador ha leido suficiente documentación acerca del lenguaje en cuestión, evitamos la burocracia y atendemos el problema de raíz en el menor tiempo posible.

La solución que propone Perl con el lema Hay más de una manera de hacerlo, es ofrecerle al programador **libertad** en su forma de expresarse, ¿acaso todos hablamos el mismo idioma?, ¿acaso debemos seguir las malas prácticas que intenta difundir el **maligno** Java?, coartar el pensar del programador y obligarlo a hacer las cosas _al estilo Java_, ¿dónde queda la imaginación? De hecho, se dice que, un programador experto en Java está muy cerca de convertirse en un autómata, ¿paso a ser un lujo?.

A todos los que lo deseen, les invito a participar en la lista de correos técnica ([l-linux](http://velug.org.ve/cgi-bin/mailman/listinfo/l-linux)) del [Grupo de Usuarios de Linux de Venezuela](http://velug.org.ve/) (VELUG), les recomiendo leer detenidamente las normas de uso antes de inscribirse en la lista.
