---
title: 'MongoDB University: M101P'
author: milmazz
date: 2014-02-17 08:00:00
tags:
  - mongodb
slug: mongodb-university-m101p
redirect_from: /archivos/2014/02/17/mongodb-university-m101p/
---

A finales del año pasado finalice satisfactoriamente el curso en línea *M101P:
MongoDB for Python Developers* de [MongoDB University][] (otrora conocido como
*10gen Education*). Sin embargo, no me había tomado el tiempo para compartirles
la experiencia, sino hasta ahora.

Cualquier persona sin práctica en el área puede tomar este curso, pues es un
abreboca al mundo detrás de MongoDB y explora un buen número de temas
relacionados a esta base de datos [NoSQL][]. Aunque está orientado a
desarrolladores en Python lo exigido por el curso es mínimo en este lenguaje,
incluso dentro de los temas está contemplado una introducción a este lenguaje.
Sin embargo, para aquellos que decidan explorar otros lenguajes de programación,
[MongoDB University][] ofrece alternativas para programadores en [Java][],
[Node.js][] y también para [DBAs][].

La duración del curso es de 7 semanas, en base al conocimiento previo que se
maneje de las tecnologías puede necesitar de unas 4 a 6 horas semanales de
dedicación. La estructura del curso permite ver inicialmente varios videos
introductorios que hacen referencia a un tópico en particular, luego podrá
reforzar lo visto con una serie de *quizzes*, en este punto siempre es
recomendable contrastar lo visto con la [documentación oficial en línea][MongoDB
Docs] y no dejar de responder todas las tareas de cada semana pues tienen un
peso total del 50%, el plazo de respuesta es más que aceptable, se da una semana
de plazo para responder. En la última semana será el examen final, el cual
consta de 10 preguntas, con ello se obtiene el otro 50%.

En mi caso aprovechaba el tiempo al ser trasladado hacia/desde el trabajo y veía
*fuera de línea* los videos ofrecidos por [MongoDB University][]. Luego, al
retornar a casa en las noches o los fines de semana respondía los *quizzes* y
las asignaciones, de ese modo me fue posible lograr aprobar sin problemas el
curso, además, al final recibes un certificado de aprobación, aunque lo más
importante para mi fue aprender algunas cuestiones que aún desconocía de
MongoDB, las semanas podrían resumirse como siguen:

En la primera semana se ve una introducción a MongoDB, se compara en cierta
medida con el esquema relacional y se plasma un bosquejo del desarrollo de
aplicaciones con MongoDB. También se explora el *shell* y se introduce la
notación JSON, a partir de acá se crean *arrays*, documentos y subdocumentos,
entre otros detalles de la [especificación][JSON Specs]. También se comienza a
preparar el ambiente de trabajo futuro, es decir, se explica como instalar
MongoDB en diversas plataformas, así como [Python][], [Bottle][]
(*microframework* para desarrollo *Web* en Python) y [pymongo][], éste último
ofrece un conjunto de herramientas para interactuar con MongoDB desde Python. Ya
para finalizar las sesiones de la semana, se da una introducción al lenguaje de
programación Python y manejo básico del *framework* Bottle.

La segunda semana se tiene mayor interacción con el *shell* de MongoDB, se
inicia el proceso de [CRUD][], se podrán ejecutar consultas por medio de `find`,
`findOne`. Se comienza el uso de [operadores de comparación][Comparison Query
Operators] de campos como [$gt][gt] y [$lt][lt], [operadores lógicos][Logical
Query Operators] como [$or][or], [$and][and], uso de operadores
[$exists][exists], [$type][type], conteo de consultas sobre colecciones con el
método [count()][count], uso de expresiones regulares (compatibles a las usadas
en Perl) a través del operador [$regex][regex][^1], consultas sobre elementos
dentro de *arrays* con los operadores [$in][in] y [$all][all], actualizaciones
de *arrays* a través de [$push][push], [$pop][pop], [$pull][pull],
[$pullAll][pullAll] y [$addToSet][addToSet]. Además, uso de operadores de
actualización [$set][set] y [$unset][unset] para establecer o eliminar campos de
un documento respectivamente. [Actualización de documentos][update] existentes y
la posibilidad de utilizar opciones como `upsert` (operación que permite hacer
una actualización si el documento en cuestión existe, de lo contrario, se
realiza una inserción automática) o `multi` (actualización múltiple). Ya para
finalizar la segunda semana se deja de lado un poco el *shell* y se comienza el
mismo proceso de interacción descrito previamente pero en esta ocasión desde
*pymongo*.

La tercera semana nos podemos encontrar con temas enfocados al área de [modelado
de datos][data-modeling], pues MongoDB ofrece un esquema flexible, incluso cada
documento puede manejar un esquema dinámico. Por lo tanto, resulta conveniente
estudiar las posibilidades existentes en el [modelaje de datos][data-models] y
que la escogencia de un esquema particular apunte a cubrir los requerimientos de
la aplicación, explotación de las características propias del motor de base de
datos y los mecanismos comunes de obtención de dichos datos. En el transcurso de
la tercera semana, se contrasta como ejemplo el modelado de datos relacional de
las tablas que pueden estar asociadas a un *blog* y como cambia dicho modelado
desde la visión no relacional, también se cubren temas como las [relaciones uno
a uno][one-to-one-relationships], [uno a muchos con referencias a otros
documentos][one-to-many-relationships], [uno a muchos con documentos
embebidos][one-to-many-relationships-between-documents], en qué casos puede ser
beneficioso embeber datos. Finalmente se explora [GridFS][GridFS] para el manejo
de *blobs*, ficheros que normalmente superen el límite de tamaño de cada
documento (16MB).

El tema principal de la cuarta semana es el rendimiento, lo cual puede estar
influenciado por muchos factores, desde el sistema de gestión de ficheros, hasta
la memoria, CPU, tipos de discos, entre otros. Sin embargo, en esta semana se
concentrarán en explorar los algoritmos que dependiendo de la situación puede
ejecutar rápidamente o no las consultas. Por lo tanto, es necesario explorar
conceptos básicos como índices, su creación y descubrimiento, índices con
múltiples claves hasta temas como la detección de posible cuellos de botella al
no utilizar índices o mala utilización de los mismos. Al finalizar se comienza a
explorar los [shardings][sharding-introduction], básicamente es una técnica que
permite dividir largas colecciones de datos en múltiples servidores.

La quinta semana se explora una de las funcionalidades mas atractivas de
MongoDB: [Aggregation framework][aggregation]. Para los que vienen del mundo SQL
puede parecerles una respuesta a la agrupación de resultados por grupos, en
donde se pueden obtener sumas (`$sum`), conteos (`$count`), promedios (`$avg`) y
otras funciones sobre un conjunto de colecciones agrupadas por ciertos valores
particulares.

La sexta semana se cubren interesantes temas como la replicación y *sharding*.
La última semana se tiene acceso a 2 casos de estudio, en particular, la primera
entrevista es con Jon Hoffman ([@Hoffrocket][Hoffrocket]) de Foursquare y la
segunda con Ryan Bubinski ([@ryanbubinski][ryanbubinski]) de Codecademy.

Después de las entrevistas viene el examen final, el cual cubre el 50% del
curso. ¡Éxitos! y recuerde siempre reforzar lo visto en el curso con la
documentación oficial, también existen recursos adicionales[^2] en la red que
podrían servirles.

Ya para finalizar quisiera decirles que se animen a intentar unirse al curso que
se ofrece de MongoDB, vale la pena intentarlo y verán que son semanas bien
aprovechadas.

[^1]: Resalto que de llegar a necesitar hacer uso de expresiones regulares a
través del operador `$regex`, tenga en cuenta que solo podrá hacer uso eficiente
del índice cuando la expresión regular incluye el ancla de inicio de cadena: ^
(acento cincunflejo) y sea *case-sensitive*.

[^2]: Puede encontrar material de apoyo introductorio en el libro [The Little
MongoDB Book][the-little-mongodb-book] de Karl Seguin, también existen guías y
presentaciones adicionales que pueden ayudarle a comprender aún más MongoDB, por
ejemplo, [esta presentación sobre distintas consultas y uso del *aggregation
framework* para explorar datos de resultados la NBA][nba-game-date].

[MongoDB University]: https://education.mongodb.com/
[Java]: https://education.mongodb.com/courses/10gen/M101J/2013_October/about
[Node.js]: https://education.mongodb.com/courses/10gen/M101JS/2013_October/about
[DBAs]: https://education.mongodb.com/courses/10gen/M102/2013_September/about
[MongoDB Docs]: http://docs.mongodb.org/manual/
[JSON Specs]: http://www.json.org/
[pymongo]: http://api.mongodb.org/python/current/
[Python]: http://python.org
[Bottle]: http://bottlepy.org
[CRUD]: http://docs.mongodb.org/manual/crud/
[gt]: http://docs.mongodb.org/manual/reference/operator/gt/
[lt]: http://docs.mongodb.org/manual/reference/operator/lt/
[and]: http://docs.mongodb.org/manual/reference/operator/and/
[or]: http://docs.mongodb.org/manual/reference/operator/or/
[count]: http://docs.mongodb.org/manual/reference/method/db.collection.count/
[all]: http://docs.mongodb.org/manual/reference/operator/all/
[in]: http://docs.mongodb.org/manual/reference/operator/in/
[addToSet]: http://docs.mongodb.org/manual/reference/operator/addToSet/
[pop]: http://docs.mongodb.org/manual/reference/operator/pop/
[pullAll]: http://docs.mongodb.org/manual/reference/operator/pullAll/
[pull]: http://docs.mongodb.org/manual/reference/operator/pull/
[push]: http://docs.mongodb.org/manual/reference/operator/push/
[regex]: http://docs.mongodb.org/manual/reference/operator/regex/
[Comparison Query Operators]: http://docs.mongodb.org/manual/reference/operator/query-comparison/
[Logical Query Operators]: http://docs.mongodb.org/manual/reference/operator/query-logical/
[exists]: http://docs.mongodb.org/manual/reference/operator/exists/
[type]: http://docs.mongodb.org/manual/reference/operator/type/
[NoSQL]: http://en.wikipedia.org/wiki/NoSQL
[set]: http://docs.mongodb.org/manual/reference/operator/set/
[unset]: http://docs.mongodb.org/manual/reference/operator/unset/
[update]: http://docs.mongodb.org/manual/reference/method/db.collection.update/
[ryanbubinski]: https://twitter.com/ryanbubinski
[Hoffrocket]: https://twitter.com/Hoffrocket
[data-modeling]: http://docs.mongodb.org/manual/core/data-modeling-introduction/
[data-models]: http://docs.mongodb.org/manual/applications/data-models/
[one-to-one-relationships]: http://docs.mongodb.org/manual/tutorial/model-embedded-one-to-one-relationships-between-documents/
[one-to-many-relationships]: http://docs.mongodb.org/manual/tutorial/model-referenced-one-to-many-relationships-between-documents/
[one-to-many-relationships-between-documents]: http://docs.mongodb.org/manual/tutorial/model-embedded-one-to-many-relationships-between-documents/
[GridFS]: http://docs.mongodb.org/manual/core/gridfs/
[the-little-mongodb-book]: https://github.com/karlseguin/the-little-mongodb-book
[nba-game-date]: http://www.slideshare.net/vkarpov15/mongodb-queries-and-aggregation-framework-with-nba-game-data
[sharding-introduction]: http://docs.mongodb.org/manual/core/sharding-introduction/
[aggregation]: http://docs.mongodb.org/manual/aggregation/
