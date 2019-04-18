---
title: 'Pylint: Análisis estático del código en Python'
author: milmazz
date: 2010-03-13 00:31:48
categories:
  - Programación
  - Python
tags:
  - Programación
  - pylint
  - Python
slug: pylint-analisis-estatico-del-codigo-en-python
redirect_from: /archivos/2010/03/13/pylint-analisis-estatico-del-codigo-en-python/
---

Básicamente el análisis estático del código se refiere al proceso de evaluación del código fuente sin ejecutarlo, es en base a este análisis que se obtendrá información que nos permita mejorar la línea base de nuestro proyecto, sin alterar la semántica original de la aplicación.

[Pylint][] es una herramienta que todo programador en Python debe considerar en su proceso de integración continua (Ej. [Bitten](http://bitten.edgewall.org/)), básicamente su misión es analizar código en Python en busca de errores o síntomas de mala calidad en el código fuente. Cabe destacar que por omisión, la guía de estilo a la que se trata de apegar Pylint es la descrita en el [PEP-8](http://www.python.org/dev/peps/pep-0008/).

Es importante resaltar que Pylint no sustituye las labores de revisión continua de alto nivel en el Proyecto, con esto quiero decir, su estructura, arquitectura, comunicación con elementos externos como bibliotecas, diseño, entre otros.

Tome en cuenta que Pylint puede arrojar falsos positivos, esto puede entenderse al recibir una alerta por parte de Pylint de alguna cuestión que usted realizó conscientemente. Ciertamente algunas de las advertencias encontradas por Pylint pueden ser peligrosas en algunos contextos, pero puede que en otros no lo sea, o simplemente Pylint haga revisiones de cosas que a usted realmente no le importan. Puede ajustar la configuración de Pylint para no ser informado acerca de ciertos tipos de advertencias o errores.

Entre la serie de revisiones que hace Pylint al código fuente se encuentran las siguientes:

  * Revisiones básicas:
    * Presencia de cadenas de documentación (_docstring_).
    * Nombres de módulos, clases, funciones, métodos, argumentos, variables.
    * Número de argumentos, variables locales, retornos y sentencias en funciones y métodos.
    * Atributos requeridos para módulos.
    * Valores por omisión no recomendados como argumentos.
    * Redefinición de funciones, métodos, clases.
    * Uso de declaraciones globales.
  * Revisión de variables:
    * Determina si una variable o `import` no está siendo usado.
    * Variables indefinidas.
    * Redefinición de variables proveniente de módulos _builtins_ o de ámbito externo.
    * Uso de una variable antes de asignación de valor.
  * Revisión de clases:
    * Métodos sin `self` como primer argumento.
    * Acceso único a miembros existentes vía `self`
    * Atributos no definidos en el método `__init__`
    * Código inalcanzable.
  * Revisión de diseño:
    * Número de métodos, atributos, variables locales, entre otros.
    * Tamaño, complejidad de funciones, métodos, entre otros.
  * Revisión de _imports_:
    * Dependencias externas.
    * _imports_ relativos o importe de todos los métodos, variables vía `*` (_wildcard_).
    * Uso de _imports_ cíclicos.
    * Uso de módulos obsoletos.
  * Conflictos entre viejo/nuevo estilo:
    * Uso de `property`, `__slots__`, `super`.
    * Uso de `super`.
  * Revisiones de formato:
    * Construcciones no autorizadas.
    * Sangrado estricto del código.
    * Longitud de la línea.
    * Uso de `<>` en vez de `!=`.
  * Otras revisiones:
    * Notas de alerta en el código como `FIXME`, `XXX`.
    * Código fuente con caracteres non-ASCII sin tener una declaración de _encoding_. [PEP-263](http://www.python.org/dev/peps/pep-0263/)
    * Búsqueda por similitudes o duplicación en el código fuente.
    * Revisión de excepciones.
    * Revisiones de tipo haciendo uso de inferencia de tipos.

Mientras Pylint hace el análisis estático del código muestra una serie de mensajes así como también algunas estadísticas acerca del número de advertencias y errores encontrados en los diferentes ficheros. Estos mensajes son clasificados bajo ciertas categorías, entre las cuales se encuentran:

 * Refactorización: Asociado a una violación en alguna _buena práctica_.
 * Convención: Asociada a una violación al estándar de codificación.
 * Advertencia: Asociadas a problemas de estilo o errores de programación menores.
 * Error: Asociados a errores de programación importantes, es probable que se trate de un _bug_.
 * Fatal: Asociados a errores que no permiten a Pylint avanzar en su análisis.

Un ejemplo de este reporte lo puede ver a continuación:

    Messages by category
    --------------------

    +-----------+-------+---------+-----------+
    |type       |number |previous |difference |
    +===========+=======+=========+===========+
    |convention |969    |1721     |-752.00    |
    +-----------+-------+---------+-----------+
    |refactor   |267    |182      |+85.00     |
    +-----------+-------+---------+-----------+
    |warning    |763    |826      |-63.00     |
    +-----------+-------+---------+-----------+
    |error      |78     |291      |-213.00    |
    +-----------+-------+---------+-----------+

Cabe resaltar que el formato de salida del ejemplo utilizado está en modo texto, usted puede elegir entre: coloreado, texto, msvs (Visual Estudio) y HTML.

Pylint le ofrece una sección dedicada a los reportes, esta se encuentra inmediatamente después de la sección de mensajes de análisis, cada uno de estos reportes se enfocan en un aspecto particular del proyecto, como el número de mensajes por categorías (mostrado arriba), dependencias internas y externas de los módulos, número de módulos procesados, el porcentaje de errores y advertencias encontradas por módulo, el total de errores y advertencias encontradas, el porcentaje de clases, funciones y módulos con _docstrings_ y su respectiva comparación con un análisis previo (si existe). El porcentaje de clases, funciones y módulos con nombres correctos (de acuerdo al estándar de codificación), entre otros.

Al final del reporte arrojado por Pylint podrá observar una puntuación general por su código, basado en el número y la severidad de los errores y advertencias encontradas a lo largo del código fuente. Estos resultados suelen motivar a los desarrolladores a mejorar cada día más la calidad de su código fuente.

Si usted ejecuta Pylint varias veces sobre el mismo código, podrá ver el puntaje de la corrida previa junto al resultado de la corrida actual, de esta manera puede saber si ha mejorado la calidad de su código o no.

    Global evaluation
    -----------------
    Your code has been rated at 7.74/10 (previous run: 4.64/10)
    If you commit now, people should not be making nasty comments about you on c.l.py

Una de las grandes ventajas de Pylint es que es totalmente configurable, además, se pueden escribir complementos o _plugins_ para agregar una funcionalidad que nos pueda ser útil.

Si usted quiere probar desde ya Pylint le recomiendo seguir la guía introductoria descrita en [Pylint tutorial](http://www.logilab.org/card/pylint_tutorial). Si desea una mayor ayuda respecto a los códigos indicados por Pylint, puede intentar el comando `pylint --help-msg=CODIGO`, si eso no es suficiente, le recomiendo visitar el wiki [PyLint Messages](http://pylint-messages.wikidot.com/) para obtener una rápida referencia de los mensajes arrojados por Pylint, organizados de varias maneras para hacer más fácil y agradable la búsqueda. Además de encontrar explicaciones de cada mensaje, especialmente útiles para aquellos programadores que recién comienzan en Python.

#### Referencias

  * [Pylint features](http://www.logilab.org/card/pylintfeatures)
  * [Pylint tutorial](http://www.logilab.org/card/pylint_tutorial)
  * [Pylint User Manual](http://www.logilab.org/card/pylint_manual)

[Pylint]: http://www.logilab.org/project/pylint
