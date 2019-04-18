---
title: Generando contraseñas aleatorias con Perl
author: milmazz
date: 2006-12-01 00:25:06
categories:
  - perl
  - Seguridad
tags:
  - cpan
  - mkpasswd
  - Perl
  - security
  - Seguridad
slug: generando-contrasenas-aleatorias-con-perl
redirect_from: /archivos/2006/12/01/generando-contrasenas-aleatorias-con-perl/
---

El día de hoy se manifestó la necesidad de generar una serie de claves aleatorias para un proyecto en el que me he involucrado recientemente, la idea es que la entrada que tenemos corresponde más o menos al siguiente formato:

    username_1
    username_2
    ...
    username_n

La salida que se desea obtener debe cumplir con el siguiente formato:

    username_1 pass_1
    username_2 pass_2
    username_3 pass_3
    ...
    username_n pass_n

En este caso debía cumplir un _requisito fundamental_, las contraseñas deben ser suficientemente seguras.

No pensaba en otra cosa que usar el lenguaje de programación [Perl](http://www.perl.com) para realizar esta tarea, así fue, hice uso del poderío que brinda [Perl](http://www.perl.com)+[CPAN](http://www.cpan.org/) y en menos de 5 minutos ya tenía la solución al problema planteado, el tiempo restante me sirvió para comerme un pedazo de torta que me dió mi hermana, quien estuvo de cumpleaños el día de ayer.

En primer lugar, debemos instalar el módulo `String::MkPasswd`, el cual nos permitirá generar contraseñas de manera aleatoria. Si usted disfruta de una distribución **decente** como [Debian](http://www.debian.org)Recuerde, Debian es _inexorable_ la instalación del módulo es _realmente_ trivial.

    # aptitude install libstring-mkpasswd-perl

Además, si usted se detiene unos segundos y lee la documentación del módulo `String::MkPasswd` Este modulo en particular no solo se encuentra perfectamente integrado con nuestra distribución favorita, sino que además sus dependencias están resueltas. Esto es una simple muestra del poderío que ofrece una distribución como Debian., se dará cuenta que la función `mkpasswd()` toma un _hash_ de argumentos opcionales. Si no le pasa ningún argumento a esta función estará generando constraseñas aleatorias con las siguientes características:

  * La longitud de la contraseña será de 9.
  * El número mínimo de dígitos será de 2.
  * El número mínimo de caracteres en minúsculas será de 2.
  * El número mínimo de caracteres en mayúsculas será de 2.
  * El número mínimo de caracteres **no** alfanuméricos será de 1.
  * Los caracteres **no** serán distribuidos entre los lados izquierdo y derecho del teclado.

Ahora bien, asumiendo que el fichero de entrada es `users.data`, la tarea con _Perl_ es resumida en una línea de la siguiente manera.

    perl -MString::MkPasswd=mkpasswd -nli -e 'print $_, " ", mkpasswd()' users.data

La línea anterior hace uso de la función `mkpasswd` del módulo `String::MkPasswd` para generar las contraseñas aleatorias para cada uno de los usuarios que se encuentran en el fichero de entrada `users.data`, además, la opción `-i`Para mayor información acerca de las distintas opciones usadas se le sugiere referirse a `man perlrun` permite editar el fichero de entrada _in situ_, ahora bien, quizá para algunos paranoicos (me incluyo) no sea suficiente todo esto, así que vamos a generar contraseñas aleatorias aún más complicadas.

{% highlight perl %}
#!/usr/bin/perl -li

use strict;
use warnings;
use String::MkPasswd qw(mkpasswd);

while(<>){
    chomp;
    print $_, " ", mkpasswd(
	    -length => 16,
	    -minnum => 5,
	    -minlower => 5,
	    -minupper => 3,
	    -minspecial => 3,
	    -distribute => 1
    );
}
{% endhighlight %}

En esta ocasión el la función `mkpasswd()` generará claves aún más complejas, dichas claves cumplirán con las siguientes condiciones.

 * `-length`: La longitud total de la contraseña, 16 en este caso.
 * `-minnum`: El número mínimo de digitos. 5 en este caso.
 * `-minlower`: El número mínimo de caracteres en minúsculas, en este caso 5.
 * `-minupper`: El número mínimo de caracterés en mayúsculas, en este caso 3.
 * `-minspecial`: El número mínimo de caracteres **no** alfanuméricos, en este caso será de 3.
 * `-distribute`: Los caracteres de la contraseña serán distribuidos entre el lado izquierdo y derecho del teclado, esto hace más díficil que un fisgón vea la contraseña que uno está escribiendo. El valor predeterminado es _falso_, en este caso el valor es _verdadero_.

El _script_ mostrado anteriormente lo podemos reducir a una línea, aunque preferí guardarlo en un fichero al que denomine `genpasswd.pl` por cuestiones de _legibilidad_.
