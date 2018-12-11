---
redirect_from: "/archivos/2006/05/24/maldito-spam-nos-invade/"
author: milmazz
comments: true
date: 2006-05-24 04:02:32
layout: post
slug: 'maldito-spam-nos-invade'
title: ¡Maldito Spam!, nos invade
categories:
- WordPress
tags:
- akismet
- bad-behavior
- spam
- spam-karma
- WordPress
---

Según las [estadísticas](http://akismet.com/stats/) del _plugin_ [Akismet](http://akismet.com/) para el día de hoy, solamente **7.961** de **523.708** son comentarios, _trackbacks_ o _pingbacks_ válidos, mientras que el resto es [Spam](http://es.wikipedia.org/wiki/Spam), eso quiere decir que aproximadamente el **1.5%** es aceptable, el resto es _escoria_.

Seguramente alguno de mis 3 lectores en este instante se estará preguntando como funciona _Akismet_, en la sección de [respuestas a preguntas frecuentes](http://akismet.com/faq/) podrá resolver esta interrogante.

> When a new comment, trackback, or pingback comes to your blog it is submitted to the Akismet web service which runs hundreds of tests on the comment and returns a thumbs up or thumbs down.

No sé que estara sucediendo con dichas pruebas últimamente, puesto que en los comentarios de mi blog han aparecido muchos _trackbacks_ escoria. Ante el abrumador aumento (se puede observar que el aumento es prácticamente exponencial según las estadísticas proporcionadas en sitio oficial de Akismet) del _spam_, muchos han decidido cerrar sus comentarios, solo basta darse una vuelta por [technorati](http://technorati.com/) bajo el [tag spam](http://technorati.com/tag/spam) y ver las acciones de algunos.

No puedo negar que el funcionamiento de Akismet en general es excelente, de hecho, antes de probar Akismet contaba con 2 ó más alternativas para combatir el _spam_, después de probarlo, no fue necesario mantener ningún otro _plugin_, pero creo que bajo esta situación es necesario comenzar a evaluar otras posibilidades que ayuden a _Akismet_.

Algunas de las cosas que podemos hacer bajo [WordPress](http://www.wordpress.org) son las siguientes:

Mejorar nuestras listas de moderación o listas negras

En el área administrativa, bajo _Opciones_ -> _Discusión_, sección Moderación de comentarios, colocar una lista de _palabras claves_ que aparezcan en estos comentarios escoria. Algunos de ellos no tienen sentido, otros te felicitan por tu trabajo, por ejemplo: Good design, Nice site, todo ese conjunto de palabras clave deben incluirse, si le gusta ser radical incluya dichas palabras clave en la sección Lista negra de comentarios, tenga cuidado si decide elegir esta última opción.

Desactivar los _trackbacks_

En mi caso, _Akismet_ ha fallado en la detección de _trackbacks_ escoria, por lo tanto, si usted quiere ser realmente radical, puede cerrarlos.

Para desactivar las entradas futuras puede ir a _Opciones_ -> _Discusión_, y desmarcar la casilla de verificación que dice Permitir notificaciones de enlace desde otros weblogs (_pingbacks_ y _trackbacks_).

Ahora bien, a pesar de que la **oleada** de _spam_ está atacando entradas recientes, podemos asegurarnos de **cerrar** los _trackbacks_ para entradas anteriores ejecutando una sencilla consulta SQL:

    UPDATE wp_posts SET ping_status = 'closed';

Moderar comentarios

Puede decidir si todos los comentarios realizados en su bitácora, deberán ser aprobados por el _administrador_, está acción quizá le evite que se muestren _trackbacks_ escoria en su sitio, pero no le evitará la **ardua** tarea de eliminarlos uno por uno o masivamente. Si quiere establecer está opción puede hacerlo desde _Opciones_ -> _Discusión_, en la sección de Para que un comentario aparezca deberá marcar la casilla de verificación Un administrador debe aprobar el comentario (independientemente de los valores de abajo)

Eliminar un rango considerable de comentarios escoria

[Esta solución](http://blindmindseye.com/2006/05/23/and-little-tip-for-the-wordpress-users-out-there/) la encontre después de buscar la manera más sencilla de eliminar cientos de mensajes _escoria_ desde una consulta en SQL.

En primer lugar debemos ejecutar la siguiente consulta:

    SELECT * FROM wp_comments ORDER BY comment_ID DESC

En ella debemos observar el rango de comentarios recientes y que sean considerados _spam_. Por ejemplo, supongamos que los comentarios cuyos ID's están entre los números 2053 y 2062 son considerados _spam_. Luego de haber anotado el rango de valores, debe proceder como sigue:

    UPDATE wp_comments SET comment_approved = ’spam’ WHERE comment_ID BETWEEN 2053 AND 2062

Recuerde sustituir apropiadamente los valores correspondientes al inicio y final de los comentarios a ser marcados como _spam_. Debe recordar también que los comentarios marcados como _spam_ no desaparecerán de su base de datos, por lo tanto, estarán ocupando un espacio que puede llegar a ser considerable, le recomiendo borrarlos posteriormente.

Utilizar _plugins_ compatibles con _Akismet_

En la sección de [respuestas a preguntas frecuentes](http://akismet.com/faq/) de Akismet podrán resolver esta interrogante.

> We ask that you turn off all other spam plugins as they may reduce the effectiveness of Akismet. Besides, you shouldn't need them anymore! :) But if you are investigating alternatives, we recommend checking out [Bad Behavior](http://www.ioerror.us/software/bad-behavior/) and [Spam Karma](http://unknowngenius.com/blog/wordpress/spam-karma/), both which integrate with Akismet nicely.

Ya había probado con anterioridad [Bad Behavior](http://www.ioerror.us/software/bad-behavior/), de hecho, lo tuve antes de probar _Akismet_ y funcionaba excelente, ahora, falta probar como se comporta al combinarlo con [Spam Karma 2](http://unknowngenius.com/blog/wordpress/spam-karma/). Las instrucciones de instalación y uso son sencillas.

## Referencias:

  * [Killing the Trackback Spam](http://www.wynia.org/wordpress/2006/05/23/killing-the-trackback-spam/).
  * [And little tip for the WordPress users out there](http://blindmindseye.com/2006/05/23/and-little-tip-for-the-wordpress-users-out-there/).
  * [Bad Behavior](http://www.homelandstupidity.us/software/bad-behavior/).
  * [Spam Karma](http://unknowngenius.com/blog/wordpress/spam-karma/).
  * [Technorati](http://technorati.com/tag/spam).
