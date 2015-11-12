---
redirect_from: "/archivos/2011/09/21/enviando-correos-con-perl/"
author: milmazz
comments: true
date: 2011-09-21 17:28:02
layout: post
slug: enviando-correos-con-perl
title: Enviando correos con Perl
wordpress_id: 408
categories:
- Perl
- Programación
tags:
- gulmer
- Perl
- Programación
- programming
- Scripts
---

Regularmente los administradores de sistemas requieren notificar, vía correo electrónico, a sus usuarios de ciertos cambios o nuevos servicios disponibles. La experiencia me ha indicado que el usuario aprecia más un correo personalizado que uno general. Sin embargo, lograr lo primero de manera manual es bastante tedioso e ineficaz. Por lo tanto, es lógico pensar en la posibilidad de automatizar el proceso de envío de correos electrónicos personalizados, en este artículo, explicaré una de las tantas maneras de lograrlo haciendo uso del lenguaje de programación [Perl](http://www.perl.org/).

En [CPAN](http://www.cpan.org/) podrá encontrar muchas alternativas, recuerde el principio [TIMTOWTDI](http://en.wikipedia.org/wiki/There_is_more_than_one_way_to_do_it). Sin embargo, la opción que más me atrajo fue `MIME::Lite:TT`, básicamente este módulo en Perl es un _wrapper_ de `MIME::Lite` que le permite el uso de plantillas, vía `Template::Toolkit`, para el cuerpo del mensaje del correo electrónico. También puede encontrar `MIME::Lite::TT::HTML` que le permitirá enviar correos tanto en texto sin formato (`MIME::Lite::TT`) como en formato HTML. Sin embargo, estoy en contra de enviar correos en formato HTML, lo dejo a su criterio.

Una de las ventajas de utilizar `Template::Toolkit` para el cuerpo del mensaje es separar en _capas_ nuestra _script_, si se observa desde una versión muy simplificada del patrón [MVC](http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller), el control de la lógica de programación reside en el _script_ en Perl, la plantilla basada en _Template Toolkit_ ofrecería la vista de los datos, de modo tal que podríamos garantizar que la presentación está separada de los datos, los cuales pueden encontrarse desde una base de datos o un simple fichero CSV. Otra ventaja evidente es el posible reuso de componentes posteriormente.

Un primer ejemplo del uso de `MIME::Lite:TT` puede ser el siguiente:

{% highlight perl %}
#!/usr/bin/perl

use strict;
use warnings;
use MIME::Lite::TT;

my %options;
$options{INCLUDE_PATH} = '/home/jdoe/example';

my %params;
$params{first_name} = "Milton";
$params{last_name}  = "Mazzarri";
$params{username}   = "milmazz";
$params{groups}     = "sysadmin";

my $msg = MIME::Lite::TT->new(
    From        => 'jdoe@example.com',
    To          => 'milmazz@example.com',
    Charset     => 'utf8',
    TimeZone    => 'America/Caracas',
    Subject     => 'Example',
    Template    => 'example.txt.tt',
    TmplOptions => \%options,
    TmplParams  => \%params,
);

$msg->send();
{% endhighlight %}

Y el cuerpo del correo electrónico, lo que en realidad es una plantilla basada en `Template::Toolkit`, vendría definido en el fichero `example.txt.tt` de la siguiente manera:

{% highlight perl %}
Hola [% last_name %], [% first_name %].

Tu nombre de usuario es [% username %].

Un saludo, feliz día.

Su querido BOFH de siempre.
{% endhighlight %}

En el _script_ en Perl mostrado previamente podemos percatarnos que los datos del destinario se encuentran inmersos en la lógica. Por lo tanto, el siguiente paso sería desacoplar esta parte de la siguiente manera:

{% highlight perl %}
#!/usr/bin/perl

use strict;
use warnings;
use MIME::Lite::TT;
use Class::CSV;

my %options;
$options{INCLUDE_PATH} = '/home/jdoe/example';

# Lectura del fichero CSV
my $csv = Class::CSV->parse(
    filename       => 'example.csv',
    fields         => [qw/last_name first_name username email/],
    csv_xs_options => { binary => 1, }
);

foreach my $line ( @{ $csv->lines() } ) {
    my %params;

    $params{first_name} = $line->first_name();
    $params{last_name}  = $line->last_name();
    $params{username}   = $line->username();

    my $msg = MIME::Lite::TT->new(
        From        => 'jdoe@example.com',
        To          => $line->email(),
        Charset     => 'utf8',
        TimeZone    => 'America/Caracas',
        Subject     => 'Example',
        Template    => 'example.txt.tt',
        TmplOptions => \%options,
        TmplParams  => \%params,
    );

    $msg->send();
}
{% endhighlight %}

Ahora los datos de los destinarios los extraemos de un fichero en formato CSV, en este ejemplo, el fichero en formato CSV lo hemos denominado `example.csv`.

Cabe aclarar que `$msg->send()` realiza el envío por medio de `Net::SMTP` y podrá usar las opciones que se describen en dicho módulo. Sin embargo, si necesita establecer una conexión SSL con el servidor SMTP es oportuno recurrir a `Net::SMTP::SSL`:

{% highlight perl %}
#!/usr/bin/perl

use strict;
use warnings;
use MIME::Lite::TT;
use Net::SMTP::SSL;
use Class::CSV;

my $from = 'jdoe@example.com';
my $host = 'mail.example.com';
my $user = 'jdoe';
my $pass = 'example';

my %options;
$options{INCLUDE_PATH} = '/home/jdoe/example';

# Lectura del fichero CSV
my $csv = Class::CSV->parse(
    filename       => 'example.csv',
    fields         => [qw/last_name first_name username email/],
    csv_xs_options => { binary => 1, }
);

foreach my $line ( @{ $csv->lines() } ) {
    my %params;

    $params{first_name} = $line->first_name();
    $params{last_name}  = $line->last_name();
    $params{username}   = $line->username();

    my $msg = MIME::Lite::TT->new(
        From        => $from,
        To          => $line->email(),
        Charset     => 'utf8',
        TimeZone    => 'America/Caracas',
        Subject     => 'Example',
        Template    => 'example.txt.tt',
        TmplOptions => \%options,
        TmplParams  => \%params,
    );

    my $smtp = Net::SMTP::SSL->new( $host, Port => 465 )
      or die "No pude conectarme";
    $smtp->auth( $user, $pass )
      or die "No pude autenticarme:" . $smtp->message();
    $smtp->mail($from)                 or die "Error:" . $smtp->message();
    $smtp->to( $line->email() )        or die "Error:" . $smtp->message();
    $smtp->data()                      or die "Error:" . $smtp->message();
    $smtp->datasend( $msg->as_string ) or die "Error:" . $smtp->message();
    $smtp->dataend()                   or die "Error:" . $smtp->message();
    $smtp->quit()                      or die "Error:" . $smtp->message();
}
{% endhighlight %}

Note en este último ejemplo que la representación en cadena de caracteres del cuerpo del correo electrónico viene dado por `$msg->as_string`.

Para finalizar, es importante mencionar que también podrá adjuntar ficheros de cualquier tipo a sus correos electrónicos, solo debe prestar especial atención en el tipo MIME de los ficheros que adjunta, es decir, si enviará un fichero adjunto PDF debe utilizar el tipo `application/pdf`, si envía una imagen en el formato GIF, debe usar el tipo `image/gif`. El método a seguir para adjuntar uno o más ficheros lo dejo para su investigación ;)
