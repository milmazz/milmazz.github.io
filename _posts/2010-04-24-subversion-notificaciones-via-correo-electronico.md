---
title: 'Subversion: Notificaciones vía correo electrónico'
author: milmazz
date: 2010-04-24 07:00:17
categories:
  - debian
  - Perl
  - Programación
  - Software
  - subversion
tags:
  - debian
  - Perl
  - Programación
  - Software
  - subversion
  - trac
slug: subversion-notificaciones-via-correo-electronico
redirect_from: /archivos/2010/04/24/subversion-notificaciones-via-correo-electronico/
---

Al darse un proceso de desarrollo colectivo es recomendable mantener una o varias listas de notificación acerca de los cambios hechos (_commits_) en el repositorio de código fuente. Para este tipo de actividades es muy útil emplear [`SVN::Notify`](http://search.cpan.org/dist/SVN-Notify/).

`SVN::Notify` le ofrece un número considerable de opciones, a continuación resumo algunas de ellas:

  * Obtiene información relevante acerca de los cambios ocurridos en el repositorio _Subversion_.
  * Realiza análisis sobre la información recolectada y brinda la posibilidad de reconocer distintos formatos vía filtros (Ej. Textile, Markdown, Trac).
  * Puede obtener la salida tanto en texto sin formato como en XHTML.
  * Le brinda la posibilidad de construir correos electrónicos en base a la salida obtenida.
  * Permite el envío de correo, ya sea por el comando `sendmail` o SMTP.
  * Es posible indicar el método de autenticación ante el servidor SMTP.

Para instalar el `SVN::Notify` en sistemas [Debian](http://www.debian.org/) o derivados proceda de la siguiente manera:

{% highlight bash %}
# aptitude install libsvn-notify-perl
{% endhighlight %}

Una vez instalado `SVN::Notify`, es hora de definir su comportamiento. Aunque es posible hacerlo vía comando `svnnotify` y empotrarlo en un _script_ escrito en _Bash_ he preferido hacerlo en [Perl](http://www.perl.org/), es más natural y legible hacerlo de este modo.

{% highlight perl %}
#!/usr/bin/perl -w

use strict;
use SVN::Notify;

my $path = $ARGV[0];
my $rev  = $ARGV[1];

my %params = (
    repos_path      => $path,
    revision        => $rev,
    handler         => 'HTML::ColorDiff',
    trac_url        => 'http://trac.example.com/project',
    filters         => ['Trac'],
    with_diff       => 1,
    diff_switches   => '--no-diff-added --no-diff-deleted',
    subject_cx      => 1,
    strip_cx_regex  => [ '^trunk/', '^branches/', '^tags/' ],
    footer          => 'Administrado por: BOFH ',
    max_sub_length  => 72,
    max_diff_length => 1024,
    from            => 'noreply@example.com',
    subject_prefix  => '[project]',
    smtp            => 'smtp.example.com',
    smtp_user       => 'example',
    smtp_pass       => 'example',
);

my $developers = 'dev@example.com';
my $admins     = 'admins@example.com';

$params{to_regex_map} = {
    $developers => '^trunk|^branches',
    $admins     => '^tags|^trunk'
};

my $notifier = SVN::Notify->new(%params);
$notifier->prepare;
$notifier->execute;
{% endhighlight %}

Si seguimos con el ejemplo indicado en el artículo anterior, [Instalación básica de Trac y Subversion](/article/2010/04/23/instalacion-basica-de-trac-y-subversion), este _hook_ lo vamos a colocar en `/srv/svn/project/hooks/post-commit`, dicho fichero deberá tener permisos de ejecución para el Servidor Web Apache.

Con este sencillo _script_ en Perl se ha logrado lo siguiente:

  * La salida se generará en XHTML.
  * Las diferencias de código serán resaltadas o coloreadas, esto es posible por el _handler_ `SVN::Notify::ColorDiff`
  * El sistema de notificación está integrado a la sintaxis de enlaces de Trac. Por lo tanto, los _commits_ que posean este tipo de enlaces serán interpretados correctamente. Ej. `#123 changeset:234 r234`

![Muestra del coloreado que ofrece SVN::Notify](/images/2010-04-24-subversion-notificaciones-via-correo-electronico/Pantallazo-271x300.png)

Aunque el código es sucinto y claro, trataré de resumir cada uno de los parámetros utilizados.

* `repos_path`: Define la ruta al repositorio _Subversion_, la cual es obtenida a partir del primer argumento que pasa _Subversion_ al ejecutar el _hook_ `post-commit`.
* `revision`: El número de la revisión del _commit_ actual. El número de la revisión se obtiene a partir del segundo argumento que pasa _Subversion_ al ejecutar el _hook_ `post-commit`
* `handler`: Especifica una subclase de `SVN::Notify` que será utilizada para el manejo de las notificaciones. En el ejemplo se hace uso de `HTML::ColorDiff`, el cual permite colorear o resaltar la sintaxis del comando `svnlook diff`
* `trac_url`: Este parámetro será usado para generar enlaces al Trac para los números de revisiones y similares en el mensaje de notificación.
* `filters`: Especifica la carga de más módulos que terminan de difinir la salida de la notificación. En el ejemplo, se hace uso del filtro `Trac`, filtro que convierte el _log_ del _commit_ que cumple con el formato de Trac a HTML.
* `with_diff`: Valor lógico que especifica si será o no incluida la salida del comando `svnlook diff` en la notificación vía correo electrónico.
* `diff_switches`: Permite el pase de opciones al comando `svnlook diff`, en particular recomiendo utilizar `--no-diff-deleted` y `--no-diff-added` para evitar ver las diferencias para los archivos borrados y añadidos respectivamente.
* `subject_cx`: Valor lógico que indica si incluir o nó el contexto del _commit_ en la línea de asunto del correo electrónico de notificación.
* `strip_cx_regex`: Acá se indican las expresiones regulares que serán utilizadas para eliminar información del contexto de la línea de asunto del correo electrónico de notificación.
* `footer`: Agrega la cadena definida al final del cuerpo del correo electrónico de notificación
* `max_sub_length`: Indica la longitud máxima de la línea de asunto del correo electrónico de notificación.
* `max_diff_length`: Máxima longitud del `diff` (esté adjunto o en el cuerpo de la notificación).
* `from`: Define la dirección de correo que será usada en la línea `From`. Si no se especifica será utilizado el nombre de usuario definido en el _commit_, esta información se obtiene vía el comando `svnlook`.
* `subject_prefix`: Define una cadena de texto que será el prefijo de la línea correspondiente al asunto del correo electrónico de notificación.
* `smtp`: Indica la dirección para el servidor SMTP que el cual se enviarán las notificaciones de correo electrónico. Si no se utiliza este parámetro, `SVN::Notify` utilizará el comando `sendmail` para el envío del mensaje.
* `smtp_user`: El nombre de usuario para la autenticación SMTP.
* `smtp_pass`: Contraseña para la autenticación SMTP
* `to_regex_map`: Este parámetro contiene un _hash_ que mantiene referencias de direcciones de correo electrónico contra expresiones regulares. La idea es enviar las notificaciones si y solo si el nombre de uno o más directorios son afectados por un _commit_ y dicha ruta coincide con las expresiones regulares definidas. Este parámetro resulta muy útil en proyectos de desarrollo de software grandes y donde es posible disponer de varias listas de correo para informar a los desarrolladores interesados en secciones específicas.
Para mayor detalle de las opciones mencionadas previamente vea [`SVN::Notify`](http://search.cpan.org/dist/SVN-Notify/), acá también encontrará más opciones de configuración.

## Observación

Hasta ahora he encontrado que el coloreado o resaltado de la sintaxis no funciona en algunos sistemas [Webmail][], como por ejemplo [Gmail][], [SquirrelMail][]. Sin embargo, en otros sistemas _Webmail_ como [RoundCube][] si funciona. Este comportamiento se presenta porque en sistemas como _Gmail_ las hojas de estilos en cascada (CSS) [internas](http://www.w3schools.com/css/css_howto.asp) no son aplicadas en la interfaz. Es por ello que en estos casos es necesario recurrir a la definición de [estilos en línea](http://www.w3schools.com/css/css_howto.asp).

[Webmail]: http://es.wikipedia.org/wiki/Webmail
[Gmail]: http://www.gmail.com
[SquirrelMail]: http://squirrelmail.org/
[RoundCube]: http://roundcube.net/
