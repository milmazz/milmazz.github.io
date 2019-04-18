---
title: edubuntu es adaptable al ambiente familiar?
author: milmazz
date: 2005-12-09 20:44:49
categories:
  - Ubuntu
tags:
  - edubuntu
  - Ubuntu
slug: edubuntu-es-adaptable-al-ambiente-familiar
redirect_from: /archivos/2005/12/09/edubuntu-es-adaptable-al-ambiente-familiar/
---

Es importante hacer notar que el objetivo primordial de [edubuntu](http://www.edubuntu.org/) es ofrecer una **alternativa** para el _ambiente escolar_ (puede ser igualmente usable por los niños en casa), ofreciendo dos modos de instalación (_servidor_ y _estación de trabajo_), el primero de los modos de instalación es ideal para ambientes escolares donde existen laboratorios, se provee [LTSP](http://ltsp.org/) (_Linux Terminal Server Project_), el cual permite que otros ordenadores (los cuales fungen como _clientes_) se conecten al _servidor_ y utilicen los recursos de éste para ejecutar sus aplicaciones de escritorio.

Lo anterior resulta muy interesante porque permite mantener todas las aplicaciones en un solo lugar (el _servidor_), cualquier actualización que se haga ocurrirá únicamente en éste. Por lo tanto, cada vez que un _cliente_ inicie sesión, automáticamente estará ejecutando un sistema actualizado.

La aclaración anterior viene dada por el artículo [Review: Is Edubuntu truly the operating system for families?](http://bloggingbaby.com/entry/1234000340071196/), redactado por [Jay Allen](http://bloggingbaby.com/), en donde el autor hace una revisión de uno de los sabores de [ubuntu](http://www.ubuntu.com), edubuntu, pensando que éste estaba orientado **exclusivamente** a los niños de la casa.

El autor intenta dar su punto de vista como un padre de familia con pocos conocimientos de informática y no como el desarrollador de software que ha sido por 12 años.

Muchos de los lectores seguramente sabrán que los sabores orientados a los usuarios de casa son: Ubuntu, [Kubuntu](http://kubuntu.org/) y quizás Xubuntu, pero no le podemos decir a un padre de familia que tiene pocos conocimientos en el área lo suguiente: Instala (X|K)ubuntu y seguidamente procede a instalar el paquete `edubuntu-desktop`. Esta opción quedará descartada en el siguiente artículo ya que desde el punto de un padre con pocos conocimientos en informática pero que esté preocupado por la educación de sus hijos preferirá tener el sistema que el realmente desea en un instante, en vez de tener que instalar ciertos paquetes para obtener lo que él en realidad necesita.

Edubuntu brinda un sistema operativo lleno de paquetes educacionales, juegos, herramientas de publicación, edición gráfica y más. Todo lo descrito previamente de manera gratuita (sin cargo alguno y comprometido con los principios del _Software Libre_ y el _Código de Fuente Abierta_), se realiza un gran trabajo para ofrecer una excelente infraestructura de accesibilidad, incluso para aquellos usuarios que no están acostumbrados al manejo de ordenadores, dentro de esa infraestructura de accesibilidad se considera el lenguaje, edubuntu brinda un sistema operativo que se adapta a cualquier usuario sin importar su lenguaje.

Edubuntu puede ser la respuesta a aquellas familias con pocos recursos económicos, en donde el tener acceso a un ordenador de altas prestaciones es un lujo, o en aquellos casos donde se tenga que pagar altas sumas de dinero anualmente por mantener un sistema operativo ineficiente y lleno de problemas.

Edubuntu le brindaría a estos niños (y familias) todo el poder y flexibilidad que ofrecen los sistemas *nix, todos estos beneficios a muy bajos costos.

He considerado importante esta revisión hecha por [Jay Allen](http://bloggingbaby.com/) acerca de edubuntu, porque no es difícil imaginar que esos millones (espero) de niños que utilizan edubuntu en sus escuelas por horas, también quisieran tener ese sistema operativo tan agradable en sus casas.

Según la opinion del autor de la revisión, Jay Allen, después de experimentar con edubuntu por un día con sus hijos Neve, Jaxon, Veda de 8, 6 y 4 años respectivamente, afirma que edubuntu aún no está totalmente listo para aquellas familias con pocos conocimientos en el campo de la tecnología, pero con las características que ofrece hasta ahora es capaz de mantener satisfechos tanto a adultos como niños.

## Puntos a favor para edubuntu

  * Edubuntu es ideal para ordenadores de bajos recursos, por ejemplo, el autor de la revisión instaló edubuntu en una máquina con procesador Celeron a 550 MHz, 17 GB de disco duro, 4 GB en un disco duro secundario y con 192 MB de RAM.
  * Respecto al punto anterior, edubuntu es capaz de brindar una mejor experiencia para los niños en un ordenador de bajos recursos que tenga instalado por ejemplo Windows 98.
  * Una vez instalado edubuntu, éste ofrece una interfaz sencilla, usable y muy rápida.
  * El sistema viene precargado con una serie de características muy amenas para los niños. De igual manera, se ofrecen características muy atractivas para los adultos de la casa, entre las que se encuentran, una suite ofimática realmente completa, herramientas para la manipulación de imágenes, visualización de películas y videos.
  * [Mozilla Firefox](http://www.mozilla.com/firefox/) es incluido por defecto en la instalación, así que si está acostumbrado a él en otros ambientes, la transición no será traumática.
  * Algunos de sus niños ni siquiera se dará cuenta del cambio, tal es el caso de Veda, el menor de los hijos de Jay Allen, ni siquiera se percató que su padre secretamente reemplazó Windows XP por edubuntu.
  * Al pasar las semanas y sus hijos se hayan acostumbrado al sistema, puede mostrarle características más avanzadas de edubuntu sin mucha dificultad, es más, seguramente ellos ya las sepan, recuerde que los niños son muy curiosos.

## Puntos en contra para edubuntu

  * ¿Cuántos padres o madres en realidad entenderán lo que significan cada uno de los modos de instalación que ofrece edubuntu?, sobretodo si éstos optan por el modo de instalación por defecto, el cual es _modo servidor_ (_server_), cuando en realidad deben elegir el _modo de estación de trabajo_ (_workstation_).
  * Muchos de los mensajes que muestra el asistente para la instalación contienen un lenguaje técnico, lo cual no es ideal en aquellos padres o madres con pocos o nulos conocimientos al respecto.
  * El asistente de instalación ofrece la creación de una única cuenta, la cual puede en cualquier instante tener permisos administrativos a través del comando `sudo` (acerca de ello explico un poco en el artículo [¿Es necesario activar la cuenta root en Ubuntu?](/article/2005/05/03/es-necesario-activar-la-cuenta-root-en-ubuntu/)), esto puede ser terrible en esos casos donde los padres o madres posean pocos o nulos conocimientos respecto a la creación de usuarios desde la cuenta predeterminada, permisos administrativos, entre otras cuestiones. Según la opinión de Jay Allen, preferiría alguna de las siguientes opciones, que el mismo asistente le dé la oportunidad de crear otra cuenta con permisos limitados o que pueda crear una cuenta para cada uno de sus hijos.
  * Otro punto en contra es el uso de nombre un tanto crípticos en la suite de educación [KDE](http://kde.org/), incluso, la hija de Jay Allen, Neve (8 años de edad), se preguntaba en voz alta el por qué de la existencia del prefijo **K** en los nombres de las aplicaciones de la suite, para ella simplemente no tenía sentido. En este caso, lo más lógico sería colocar nombres más explicativos en las aplicaciones. Por lo tanto, no tendría que ser necesario abrir una aplicación solamente para saber de que trata o que soluciones puede brindarle.
  * Otro punto en contra es el hecho que el programa Mozilla Firefox no traiga por _defecto_ una sección que permita de cierta manera controlar la navegación de sus hijos, para evitar que estos últimos vean contenido explícito para adultos. Respecto a este punto en particular no es responsabilidad de edubuntu como tal, debido a que el nombre Firefox es una _marca registrada_ (esto puede ser conveniente para el control de calidad de la aplicación como tal), a pesar que el proyecto de la Fundación Mozilla es código abierto, no se pueden realizar cambios a la aplicación original y distribuirla bajo el mismo nombre. Por lo tanto, los desarrolladores de edubuntu deben elegir entre dejar de usar el reconocido nombre Firefox, o no incluir ninguna extensión útil. Más adelante se darán detalles acerca de como solucionar este problema.

## Propuestas

De la revisón hecha por Jay Allen, han salido algunas propuestas, entre ellas, una de las que me llamó la atención fué la de distribuir de manera separada los dos modos de instalación de edubuntu, lo anterior sería ideal en esos casos (que deben ser cientos) donde el padre (o madre) no posea muchos conocimientos acerca del tema, éste no sabrá que opción elegir, cuando él (o ella) ni siquiera sabe lo que significa instalación en _modo servidor_ o _modo estación de trabajo_. También podría considerarse cambiar el modo de instalación por defecto, en vez de ser _servidor_, cambiarlo por _estación de trabajo_.

Otra propuesta interesante es la necesidad de proporcionar tutoriales interactivos que de alguna manera introduzcan a los niños de diversas edades en las características, capacidades y beneficios que les proporciona el sistema que manejan.

Si edubuntu no quiere prescindir del conocido nombre Firefox, debe facilitar después de la instalación una guía que indique los pasos necesarios para establecer el control de la navegación de los niños. Sería ideal colocar una guía para instalar dicha extensión o _plugin_ en la página inicial de Firefox, o proporcionar un acceso directo a esta guía de "Primeros Pasos" desde el escritorio de la cuenta predeterminada.
