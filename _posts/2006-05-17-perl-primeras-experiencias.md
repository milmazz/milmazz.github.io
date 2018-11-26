---
redirect_from: "/archivos/2006/05/17/perl-primeras-experiencias/"
author: milmazz
comments: true
date: 2006-05-17 04:10:29
layout: post
slug: perl-primeras-experiencias
title: 'Perl: Primeras experiencias'
categories:
- Perl
tags:
- cpan
- debian
- entities
- feed
- html
- Perl
- scripting
---

La noche del sábado pasado, después de terminar de estudiar con Ana la cátedra Programación Paralela y Distribuida, me dispuse a revisar las distintas instancias de [Planeta Linux](http://planetalinux.org), como normalmente hago, pero eso no fué suficiente, me puse a validar el _feed_ que se estaba generando en ese momento (en particular, el _feed_ de la [instancia venezolana](http://ve.planetalinux.org/)). Me percate de varios errores y advertencias, entre ellos me llamo la atención:

> This feed does not validate.
>
> In addition, this feed has an issue that may cause problems for some users. We recommend fixing this issue.
>
>
> line 11, column 71: **title** should not contain HTML (20 occurrences) [[help](http://validator.w3.org/feed/docs/warning/ContainsHTML.html)]

Luego de leer la ayuda noto que es recomendable cambiar todos los nombres de las entidades html a su equivalente decimal, es decir, si tenemos por ejemplo: `&copy;` debe modificarse `&#169;`, de igual manera con el resto.

Tenía varias opciones, una de ellas era realizar el reemplazo masivo desde [vim](http://www.vim.org/), pero esta tarea es realmente ineficiente por el hecho de tener que reemplazar todos los nombres de las entidades html a su equivalente decimal uno por uno, además de eso, debía hacerlo para las tres instancias presentes en Planeta Linux, primera opción descartada de entrada.

Aprovechando que la semana pasada, al igual que el profesor [Francisco Palm](http://ieac.faces.ula.ve/mapologo/), estuve presente en un curso sobre el lenguaje de programación [Perl](http://perl.com), el cual fué dictado por José Luis Rey con la ayuda de Daniel Rodríguez en las instalaciones de [Fundacite Mérida](http://www.funmrd.gov.ve/), quería poner en práctica algunas de las cosas que aprendí en dicho curso.

Antes de continuar debo agradecer al profesor José Aguilar, a la ingeniera Blanca Abraham, y a la Sra. Tauka Shults por la oportunidad que me brindaron.

Una de las cosas que nos recalcó José Luis fué acerca de las virtudes que debía tener un programador, una de ellas debe ser la _flojera_, es decir, comenzar a escribir código realmente útil de inmediato, sin ningún requerimiento adicional como ocurre en lenguajes de programación como el C/C++, en donde es necesario realizar una serie de procedimientos **antes** de comenzar a escribir código **útil**.

Como mencione en el párrafo anterior, la idea es llegar a ser lo más productivo en el menor tiempo posible. Generar un _hash_ de entidades de nombres html no me parecía el camino idóneo, así que recorde el tema de la **flojera**, sin pensarlo dos veces comence a buscar en [search.cpan.org](http://search.cpan.org/) un módulo que me permitiera convertir los nombres de las entidades _html_ a su equivalente decimal, como **primer** resultado obtuve lo que buscaba, el módulo [HTML::Entities::Numbered](http://search.cpan.org/~taniguchi/HTML-Entities-Numbered-0.04/lib/HTML/Entities/Numbered.pm), había encontrado mi salvación, leo un poco acerca de su uso y es más sencillo de lo que pensaba, siguiente paso, proceder a instalarlo.

Para _debianizar_ un módulo en Perl es muy sencillo, en primer lugar debemos recurrir al comando `dh-make-perl`, si no lo tenemos instalado ya, debemos proceder como sigue:

    # aptitude install dh-make-perl

Ahora ya podemos _debianizar_ el módulo que requerimos, para ello tuve que realizar lo siguiente:

    # dh-make-perl --build --cpan HTML::Entities::Numbered

Como lo puede apreciar, su uso es realmente sencillo, para una mejor explicación acerca de este último comando le recomiendo leer la entrada [Instalando módulos de Perl en Debian](http://g013m.unplug.org.ve/?p=26) escrita por [Christian Sánchez](http://g013m.unplug.org.ve/), como era la primera vez que hacia uso del comando `cpan` tuve que configurarlo, esto no tomo mucho tiempo, el asistente ofrece explicaciones bastante detalladas.

Una vez realizado el proceso más _complicado_ de toda la operación, el resto era escribir el código fuente que me permitiese convertir los nombres de las entidades html a su equivalente decimal, he aquí el resultado.

    #!/usr/bin/perl -l

    use strict;
    use warnings;
    use HTML::Entities::Numbered;

    unless(open(INPUT, $ARGV[0])) { die "ERROR: No se especifico archivo para abrir. $!"; }
    open(OUTPUT, ">$ARGV[0].bak");
    while(<INPUT>){ print OUTPUT name2decimal($_) if chomp; }

**¡Listo!**, en tan pocas líneas de código he logrado resolver el problema, por supuesto, todo se redujo a buscar el módulo apropiado, una vez hecho los cambios a los ficheros de configuración de Planeta Linux procedí a actualizar la última versión en [subversion](http://subversion.tigris.org/).

Puede apreciar el [antes](/article/2006/05/17/perl-primeras-experiencias/antes/) y [después](/article/2006/05/17/perl-primeras-experiencias/despues/) de los cambios realizados.
