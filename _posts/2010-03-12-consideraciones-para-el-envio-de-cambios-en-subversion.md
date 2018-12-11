---
redirect_from: "/archivos/2010/03/12/consideraciones-para-el-envio-de-cambios-en-subversion/"
author: milmazz
comments: true
date: 2010-03-12 01:48:02
layout: post
slug: consideraciones-para-el-envio-de-cambios-en-subversion
title: Consideraciones para el envío de cambios en Subversion
categories:
- Programación
- subversion
tags:
- Ingeniería del Software
- Programación
- subversion
---

Hoy día pareciese que los sistemas de control de versiones centralizados están pasando de moda ante la aparición de sistemas de control de versiones descentralizados poderosos como [Git](http://git-scm.com/), del cual espero poder escribir en próximos artículos. Sin embargo, puede que la adopción de estos sistemas descentralizados tarden en controlar el mundo de los SCM, al menos por ahora las métricas de [ohloh.net](http://www.ohloh.net) indican que [Subversion](http://subversion.tigris.org/) sigue siendo bastante empleado, mayor detalle en el artículo [Subversion - As Strong As Ever](http://blogs.open.collab.net/svn/2009/11/subversion-as-strong-as-ever.html).

En este artículo se expondrán algunas políticas que suelen definirse para la sana convivencia entre colaboradores de un proyecto de desarrollo de software, estas reglas no solo aplican a Subversion en particular, son de uso común en otros sistemas de control de versiones centralizados.

## Analice bien los cambios hechos antes de enviarlos

Cualquier cambio en la línea base de su proyecto puede traer consecuencias, tome en cuenta que los demás desarrolladores obtendrán sus cambios una vez que estén en el repositorio centralizado. Si usted no se preocupó por validar que todo funcionara correctamente antes de enviar sus cambios, _es probable que algún compañero le recrimine por su irresponsabilidad_, esto afecta de cierta forma el anillo de confianza entre los colaboradores del proyecto.

## Nunca envíe cambios que hagan fallar la construcción del proyecto

Siempre verifique que su proyecto compile, si el proyecto presenta errores debido a los cambios hechos por usted atiendalos inmediatamente, evite los errores de sintaxis, tenga siempre presente respetar las políticas de estilo de código definidas para su proyecto.

Si ha ingresado nuevos ficheros o directorios al proyecto recuerde ejecutar el comando `svn add` para programar la adición en Subversion, si omite este paso es posible que su copia de trabajo funcione o compile, pero la del resto de sus compañeros no, _evite de nuevo ser recriminado por los demás colaboradores del proyecto_.

Si su proyecto pretende ser multiplataforma, trate de imaginar las consecuencias de sus cambios bajo otro sistema operativo o arquitectura. Por ejemplo, he visto más de una vez este tipo de error:

{% highlight python %}
path = "dir/subdir/fichero.txt" # Malo
path = os.path.join(os.path.dirname(__file__), 'dir', 'subdir', 'fichero.txt') # Correcto
{% endhighlight %}

### Pruebe los cambios antes de hacer el envío

Antes de realizar un envío al repositorio centralizado actualice su copia de trabajo (`svn up`), verifique que las pruebas unitarias, regresión de su proyecto arrojan resultados positivos, de igual manera haga pruebas funcionales del sistema.

Al momento de hacer la actualización de su copia de trabajo tome nota de los ficheros editados por los demás colaboradores del proyecto, verifique que no existan conflictos, nuevos ficheros que no haya considerado, entre otros. Una vez que haya solventado la actualización de su copia de trabajo, construya (su proyecto tiene un sistema de autoconstrucción, ¿verdad?) el paquete, inicie su aplicación que contiene los cambios locales y asegúrese que el comportamiento obtenido sea igual al esperado.

## Promueva un histórico de cambios descriptivo

El histórico de cambios debe ser comprensible por cualquier colaborador del proyecto solamente con la información suministrada en dicho registro, evite depender de información fuera del contexto del envío de cambios. Se le recomienda colocar toda la información importante que no pueda obtenerse a partir de un `svn diff` del código fuente.

## ¿Está corrigiendo un error?

Si usted está reparando un error en la aplicación que se encuentra presente en la rama principal de desarrollo (_trunk_), considere seriamente portar esa reparación a otras ramas de desarrollo (_branches_) en el caso que su proyecto posea una versión estable que requiere actualmente de mantenimiento. Trate en lo posible de aprovechar el mismo _commit_ para enviar la corrección de la rama principal de desarrollo (_trunk_) y las ramas de mantenimiento.

Si el error fue reportado a través del sistema de seguimiento de errores (usted mantiene un sistema de seguimiento de errores, ¿verdad?), agregue en el registro del mensaje de envío el número del ticket, boleto o reporte que usted está atendiendo. Seguidamente proceda a cerrar el ticket o boleto en el sistema de seguimiento de errores.

## No agregue ficheros generados por otras herramientas al repositorio

No agregue ficheros innecesarios al repositorio, el origen de estos ficheros suele ser:

  * Proceso de autoconstrucción (Ej. distutils, autotools, scons, entre otros).
  * Herramientas de verificación de calidad del código (Ej. PyLint, CppLint, entre otros).
  * Herramientas para generar documentación (Ej. Doxygen, Sphinx, entre otros).

Estos ficheros generados por otras herramientas no es necesario versionarlos, puede considerarlos _cache_, este tipo de datos son localmente generados como resultado de operaciones I/O o cálculos, las herramientas deben ser capaces de regenerar o restaurar estos datos a partir del código presente en el repositorio central.

## Realice envíos atómicos

Recuerde que SVN le brinda la posibilidad de enviar más de un fichero en un solo _commit_. Por lo tanto, envíe todos los cambios relacionados en múltiples ficheros, incluso si los cambios se extienden a lo largo de varios directorios en un mismo _commit_. De esta manera, se logra que SVN se mantenga en un estado compatible antes y después del _commit_.

Recuerde asegurarse al enviar un cambio al repositorio central reflejar un solo propósito, el cual puede ser la corrección de un error de programación o bug, agregar una nueva funcionalidad al sistema, ajustes de estilo del código o alguna tarea adicional.

## Separe los ajustes de formato de código de otros envíos

Ajustar el formato del código, como la sangría, los espacios en blanco, el largo de la línea incrementa considerablemente los resultados del `diff`, si usted mezcla los cambios asociados al ajuste de formato de código con otros estará dificultando un posterior análisis al realizar un `svn diff`.

## Haga uso intensivo de la herramienta de seguimiento de errores

Trate en lo posible de crear enlaces bidireccionales entre el conjunto de cambios hechos en el repositorio de subversion y la herramienta de seguimientos de errores tanto como sea posible.

  * Trate de hacer una referencia al _id_ o número del _ticket_ al cual está dando solución desde su mensaje de registro o _log_ previo a realizar el _commit_.
  * Cuando agregue información o responda a un _ticket_, ya sea para describir su avance o simplemente para cerrar el _ticket_, indique el número o números de revisión que atienden o responden a dichos cambios.

## Indique en el registro de envío el resultado del merge

Cuando usted está enviando el resultado de un _merge_, asegúrese de indicar sus acciones en el registro de envío, tanto aquello que fue fusionado como los números de revisiones que fueron tomadas en cuenta. Ejemplo:

    Fusión de las revisiones 10:40 de /branches/foo a /trunk.

## Tenga claro cuando es oportuno crear una rama

Esto es un tema polémico. Sin embargo, dependiendo del proyecto en el que esté involucrado usted puede definir una estrategia o mezcla de ellas para manejar el desarrollo del proyecto. Generalmente puede encontrar lo siguiente:

Los proyectos que requieren un alto manejo y supervisión recurren a estrategias de siempre crear ramas, acá puede encontrarse que cada colaborador crea o trabaja en una rama privada para cada tarea de codificación. Cuando el trabajo individual ha finalizado, algún responsable, ya sea el fundador del proyecto, un revisor técnico, analiza los cambios hechos por el colaborador y hace la fusión con la línea principal de desarrollo. En estos casos la rama principal de desarrollo suele ser bastante estable. Sin embargo, este modo de trabajo conlleva normalmente a crear mayores conflictos en la etapa de fusión o _merge_, además, las labores de fusión bajo este esquema son constantemente requeridas.

Existe otro enfoque que permite mantener una línea base de desarrollo estable, el cual consiste en crear ramas de desarrollo solo cuando se ameritan. Sin embargo, esto requiere que los colaboradores tengan mayor dominio del uso de Subversion y una constante comunicación entre ellos. Además de aumentar el número de pruebas antes de cada envío al repositorio centralizado.

Estudie, analice las distintas opciones y defina un método para la creación de ramas en su proyecto. Una vez definido, es sumamente importante que cada colaborador tenga clara esta estrategia de trabajo.

Las estrategias antes mencionadas y otras más pueden verse en detalle en:

  * [Branching Strategy Questioned](http://blogs.open.collab.net/svn/2007/11/branching-strat.html).
  * [Software Branching and Parallel Universes](http://www.codinghorror.com/blog/2007/10/software-branching-and-parallel-universes.html).
  * [Branching and Merging](http://svnbook.red-bean.com/en/1.1/ch04.html)

## Los sistemas de control de versiones no substituyen la comunicación entre los colaboradores

Por último pero no menos importante, los sistemas de control de versiones no substituyen la comunicación entre los desarrolladores o colaboradores del proyecto de software. Cuando usted tenga planes de hacer un cambio que pueda afectar una cantidad de código considerable en su proyecto, establezca un control de cambios, haga un análisis de las posibles consecuencias o impacto de los mismos, difunda esta información a través de una lista de discusión (usted mantiene una lista de discusión para desarrolladores, ¿verdad?) y espere la respuesta, preocupaciones, sugerencias de los demás colaboradores, quizá juntos se encuentre un modo más eficaz de aplicar los cambios.

## Referencias:

  * [Subversion Best Practices](http://www.slideshare.net/mza/subversion-best-practices) (presentación)
  * [SVN commit basic policy guide](http://www.marten-online.com/source-versioning/svn-commit-basic-policy-guide.html)
  * [Subversion Best Practices](http://svn.apache.org/repos/asf/subversion/trunk/doc/user/svn-best-practices.html)
  * [Subversion Best Practices](http://www.red-bean.com/fitz/presentations/2006-06-28-AC-EU-Subversion-best-practices.pdf) (presentación)
