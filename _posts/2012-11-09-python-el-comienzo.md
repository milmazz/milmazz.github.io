---
redirect_from: "/archivos/2012/11/09/python-el-comienzo/"
author: milmazz
comments: true
date: 2012-11-09 22:22:20
last_modified_at: 2014-02-07
layout: post
slug: python-el-comienzo
title: 'Python: El comienzo'
categories:
- Programación
- Python
---

### ¿Qué es Python?

-   Lenguaje de programación orientado a objetos, interactivo e
    interpretado.
-   Estructuras de datos alto nivel, dinámicamente tipado.
-   Aumenta la productividad de desarrollo en comparación con otros
    lenguajes estáticamente tipados.
-   Multiplataforma:
    -   El interprete **CPython** puede ejecutarse nativamente en
        muchas plataformas (esto incluye Windows, OS X, GNU/Linux, entre
        [otras](http://www.python.org/download/other/)).
    -   [Jython](http://www.jython.org/) corre dentro de la *Java
        Virtual Machine*.
    -   [IronPython](http://ironpython.codeplex.com/) está orientado a
        plataformas **.NET** y **Mono**.
    -   [PyPy](http://pypy.org/) es una implementación enfocada en el
        [rendimiento](http://speed.pypy.org/) con un compilador *Just In
        Time*
    -   [Stackless](http://www.stackless.com/) es una rama de CPython
        con soporte a *microthreads*

[Python](http://www.python.org) es un lenguaje dinámicamente tipado, esto quiere
decir que el tipo de cada variable se conoce en tiempo de ejecución, a su vez,
[Python](http://www.python.org) es un lenguaje fuertemente tipado, lo anterior
implica que si usted posee una variable de tipo entero no puede tratarla como
una cadena de texto a menos que usted realice la conversión de manera explícita
previamente.

**CPython** es la primera implementación del lenguaje
[Python](http://www.python.org), también es la de mayor uso a nivel mundial,
dicha implementación esta escrita en el lenguaje de programación **C**. Esta es
la implementación que todos conocemos al referirnos al lenguaje de programación
[Python](http://www.python.org), pero en aquellos escenarios en donde es
necesario distinguirla de otras implementaciones alternativas se le conoce como
**CPython**.

### ¿Por qué usar Python?

En uno de los primeros libros que leí sobre Python, [Mark
Lutz](http://rmi.net/~lutz/) resumía estos puntos de la siguiente manera:

#### Calidad del software

Para muchos, Python se enfoca en la legibilidad, coherencia, y la calidad del
software en general. Al ofrecer un diseño legible, el proceso de mantenimiento
se hace más fácil y rápido.

#### Incremento en la productividad del desarrollo

El código en Python regularmente es de 1/3 a 1/5 el tamaño de un código
equivalente escrito en C++ o Java. Esto quiere decir que el desarrollador tendrá
menos que escribir, menos que depurar, y menos que mantener después de todo.
Además, los programas en Python pueden ejecutarse inmediatamente, sin tener que
recurrir a los largos y tediosos pasos de compilación y enlazado.

#### Portabilidad

La mayoría de los programas en Python se pueden ejecutar indistintamente en la
mayoría de las plataformas. Por otra parte, [Python](http://www.python.org) le
ofrece múltiples opciones para desarrollar interfaces gráficas de usuario
portables, entre ellas contamos con: TkInter, wxPython, PyGTK, PyQT, WxWidgets.
Además, cuenta con módulos de accesos a base de datos, sistemas Web, entre
otros.

#### Integración de componentes

Los scripts en Python fácilmente pueden comunicarse con otras partes de una
aplicación, usando una variedad de mecanismos para la integración. Actualmente,
Python puede invocar librerías en C y C++, puede ser llamado desde programas en
C y C++, puede integrarse con componentes en Java, puede comunicarse sobre Corba
y .NET, puede interactuar en la red con interfaces como SOAP, protocolo estándar
que define cómo dos objetos en diferentes procesos puede comunicarse por medio
del intercambio de datos en XML, REST y XML-RPC.

#### Diversión

Ya que Python es tan fácil de usar y le brinda un gran conjunto de herramientas,
puede hacer el acto de programar una tarea muy placentera.

### ¿Qué es el Zen de Python?

A través de *Zen de Python* puede conocer los principios del diseño y la
filosofía detrás del lenguaje de programación Python. También resulta útil para
comprender y utilizar el lenguaje.

{% highlight python linenos %}
    >>>  import this
    The Zen of Python, por Tim Peters
{% endhighlight %}

Obtendrá por respuesta algo como:

    Hermoso es mejor que Feo.
    Explícito es mejor que Implícito.
    Simple es mejor que Complejo.
    Complejo es mejor que Complicado.
    Lineal es mejor que Cruzado.
    Escaso es mejor que Denso.
    Poder leer el código, cuenta.
    Los Casos Especiales no son tan especiales como para romper reglas.
    Lo Práctico le gana a Lo Puro.
    Los Errores no deben pasar desapercibidos.
    A menos de que Explícitamente se silencien.
    En la ambigüedad, rehusa el Adivinar.
    Sólo debería haber una, y preferiblemente Una, manera
    obvia de hacer las cosas.
    Aunque esa manera no se obvia al principio, a menos que seas (Dutch).
    Ahora es mejor que Nunca.
    A pesar de que Nunca es mejor que Ahora Mismo.
    Si la implementación es difícil de explicar, es probable
    que sea una mala idea.
    Si la implementación es fácil de explicar, es probable
    que sea una buena idea.
    Namespaces son una Gran idea — Hagamos mas de esas.

### ¿Qué tipos de aplicaciones puedo desarrollar en Python?

Python es un lenguaje multipropósito, basta darle una revisión al
[Python Package Index](http://pypi.python.org/pypi) para ver una muestra
de las aplicaciones desarrolladas con este hermoso lenguaje.

Sin embargo, podemos ubicar el dominio de las aplicaciones en Python en:

#### Herramientas y utilidades para la administración de sistemas

Python provee interfaces para servicios de Sistemas Operativos, desde un
programa escrito en Python podemos buscar ficheros y árboles de directorios,
cargar otros programas, realizar procesamiento paralelo, entre otras cosas muy
interesantes.

#### GUI (Interfaz gráfica de usuario)

Python viene con una interfaz estándar orientada por objetos llamada
[TkInter](http://wiki.python.org/moin/TkInter), la cual permite a los programas
escritos en Python implementar GUIs con una interfaz nativa del Sistema
Operativo. Además, pueden encontrarse implementaciones más atractivas como:
[wxWidgets](http://www.wxpython.org/), [GTK+](http://www.pygtk.org/), Qt
([pyqt](http://www.riverbankcomputing.co.uk/software/pyqt/intro) o
[pyside](http://www.pyside.org/)), entre otras.

#### Internet

Python viene con módulos estándar para Internet que permiten a los programas
realizar una gran variedad de tareas en redes, tanto en el modo servidor como
cliente. Los *scripts* en Python pueden comunicarse sobre sockets; transferir
ficheros vía FTP; procesar ficheros XML, JSON, también permite establecer
comunicaciones sobre XML-RPC, SOAP, telnet, [hermosas APIs para la interacción
con [HTTP](http://docs.python-requests.org/en/latest/ "Requests: HTTP for
Humans") y [más](http://docs.python.org/2/library/internet).

En el ámbito Web se pueden encontrar variadas opciones, desde [CGI
básicos](http://wiki.python.org/moin/CgiScripts), pasando por [avanzados
manejadores de contenido](http://www.plone.org/) hasta *frameworks* de
desarrollo como [Django](http://www.djangoproject.com/),
[Flask](http://flask.pocoo.org/) y *microframeworks* como
[Bottle](http://bottlepy.org/).

#### Bases de Datos

Python ofrece módulos para interactuar con la mayoría de las bases de datos
relacionales, existen interfaces en Python: PostgreSQL, MySQL, Sybase, Oracle,
Informix, ODBC, y más. También ofrece un [API estándar para la interacción con
Bases de Datos](http://www.python.org/dev/peps/pep-0249/). Es claro entonces
poder observar [proyectos que brindan una interacción de alto nivel con bases de
datos](http://wiki.python.org/moin/HigherLevelDatabaseProgramming) como los ORM
[SQLAlchemy](http://www.sqlalchemy.org/) o
[DAL](http://web2py.com/books/default/chapter/29/06).

También hay cabida para el mundo NoSQL en Python, donde hay soporte para:
Cassandra, Riak, MongoDB, CouchDB y Redis por mencionar solo algunas.

#### Juegos, Imágenes, AI, XML

Se pueden realizar fácilmente gráficos y programación de juegos en
Python con la librería [pygame](http://pygame.org/),
[PyKyra](http://www.alobbs.com/pykyra),
[pyglet](http://www.pyglet.org/), [cocos2d](http://cocos2d.org/) y
[más](http://wiki.python.org/moin/PythonGames), incluso puedes
participar en el concurso [PyWeek](http://www.pyweek.org/).

También se puede incluir en el largo listado el procesamiento de imágenes con el
paquete PIL y otros. Programación de Inteligencia Artificial con simuladores de
redes neuronales y Sistemas Expertos, análisis de ficheros XML con el paquete de
bibliotecas xml. Python soporta a traves de extensiones Programación numérica
avanzada, NumPy/SciPy vuelve a Python en una sofisticada herramienta de fácil
uso para la [programación numérica y
científica](http://wiki.python.org/moin/NumericAndScientific). De manera
adicional, Python puede soportar animaciones,[visualización
2D](http://matplotlib.org/) y [3D](http://www.vrplumber.com/py3d.py), [Biología
Molecular](http://www.onlamp.com/pub/a/python/2002/10/17/biopython.html),
cálculo estadístico (RPy) y más.

### ¿Quienes usan Python?

La lista incluye a grandes en el mundo de tecnología, un excelente
resumen se puede encontrar en el listado de [casos de
éxito](http://python.org/about/success) y
[citas](http://python.org/about/quotes) del portal oficial de Python.

¿Cómo puedo mantenerme al día?
==============================

Son muchas las fuentes de información sobre Python, algunas interesantes son las
siguientes:

-   [Sitio oficial de Python](http://www.python.org)
-   [Lista de discusión de Python](http://mail.python.org)
-   [Planet Python](http://planet.python.org/)
-   [Preguntas de uso frecuente acerca de
    Python](http://www.python.org/doc/faq)
-   [Tutorial oficial de Python](http://www.python.org/doc/tut)
-   Newsletters: [Pycoder's Weekly](http://pycoders.com/), [Python
    Weekly](http://www.pythonweekly.com/)

En el siguiente artículo de esta serie espero poder listar algunos recursos que
pueden ser de ayuda a los principiantes en este fabuloso mundo de la
programación con Python.
