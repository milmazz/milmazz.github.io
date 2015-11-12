---
redirect_from: "/archivos/2010/11/17/generar-reporte-en-formato-csv-de-tickets-en-trac-desde-perl/"
author: milmazz
comments: true
date: 2010-11-17 17:12:06
layout: post
slug: generar-reporte-en-formato-csv-de-tickets-en-trac-desde-perl
title: Generar reporte en formato CSV de tickets en Trac desde Perl
wordpress_id: 355
categories:
- Perl
- Programación
tags:
- csv
- Perl
- Programación
- trac
---

El día de hoy recibí una llamada telefónica de un compañero de labores en donde me solicitaba con cierta preocupación un "pequeño" reporte del estado de un listado de _tickets_ que recién me había enviado vía correo electrónico puesto que no contaba con conexión a la _intranet_, al analizar un par de _tickets_ me dije que no iba a ser fácil realizar la consulta desde el asistente que brinda el mismo [Trac][]. Así que inmediatamente puse las manos sobre un pequeño _script_ en Perl que hiciera el _trabajo sucio_ por mí.

Es de hacer notar que total de tickets a revisar era el siguiente:

	$ wc -l tickets
	126 tickets

Tomando en cuenta el resultado previo, era **inaceptable** hacer dicha labor de manera manual. Por lo tanto, confirmaba que realizar un _script_ era la vía correcta y a la final iba a ser más divertido.

Tomando en cuenta que el formato de entrada era el siguiente:

	#3460
	#3493
	...

El formato de la salida que esperaba era similar a la siguiente:

	3460,"No expira la sesión...",closed,user

Básicamente el formato implica el _id_, _sumario_, _estado_ y _responsable_ asociado al **ticket**.

[Net::Trac][] le ofrece una manera sencilla de interactuar con una instancia remota de Trac, desde el manejo de credenciales, consultas, revisión de _tickets_, entre otros. A la vez, se hace uso del módulo [Class::CSV][] el cual le ofrece análisis y escritura de documentos en formato CSV.

{% highlight perl %}
#!/usr/bin/perl 

use warnings;
use strict;

use Net::Trac;
use Class::CSV;

# Estableciendo la conexion a la instancia remota de Trac
my $trac = Net::Trac::Connection->new(
    url      => 'http://trac.example.com/project',
    user     => 'user',
    password => 'password'
);

# Construccion del objecto CSV y definicion de opciones
my $csv = Class::CSV->new(
    fields         => [qw/ticket sumario estado responsable/],
    line_separator => "\r\n",
    csv_xs_options => { binary => 1, }    # Manejo de caracteres non-ASCII
);

# Nos aseguramos que el inicio de sesion haya sido exitoso
if ( $trac->ensure_logged_in ) {
    my $ticket = Net::Trac::Ticket->new( connection => $trac );

    # Consultamos cada uno de los tickets indicados en el fichero de entrada
    while ( my $line = <> ) {
        chomp($line);
        if ( $line =~ m/^#\d+$/ ) {
            $line =~ s/^#(\d+)$/$1/;
            $ticket->load($line);

            $csv->add_line(
                {
                    ticket      => $ticket->id,
                    sumario     => $ticket->summary,
                    estado      => $ticket->status,
                    responsable => $ticket->owner
                }
            );
        }
        else {
            print "[INFO] La linea no cumple el formato requerido: $line\n";
        }
    }
    $csv->print();
}
else {
    print "No se pudieron asegurar las credenciales";
}
{% endhighlight %}

La manera de ejecutar el `script` es la siguiente:

	$ perl trac_query.pl tickets

En donde `trac_query.pl` es el nombre del _script_ y `tickets` es el fichero de entrada.

Debo aclarar que el _script_ carece de comentarios, _mea culpa_. Además, el manejo de opciones vía linea de comandos es _inexistente_, si desea mejorarlo puede hacer uso de [Getopt::Long][].

Cualquier comentario, sugerencia o corrección es bienvenida.

[Trac]: http://trac.edgewall.com
[Net::Trac]: http://search.cpan.org/~jesse/Net-Trac-0.15/lib/Net/Trac.pm
[Class::CSV]: http://search.cpan.org/~djr/Class-CSV-1.03/CSV.pm
[Getopt::Long]: http://perldoc.perl.org/Getopt/Long.html
