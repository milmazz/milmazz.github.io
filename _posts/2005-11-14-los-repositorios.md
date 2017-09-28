---
redirect_from: "/archivos/2005/11/14/los-repositorios/"
author: chantanito
comments: true
date: 2005-11-14 14:33:59
layout: post
slug: los-repositorios
title: Los Repositorios
categories:
- debian
- Ubuntu
tags:
- debian
- Ubuntu
---

## Contenido:

  1. Definición
  2. ¿Cómo funcionan los Repositorios?
  3. ¿Cómo establecer Repositorios?
    1. Los Repositorios Automáticos
    2. Los Repositorios Triviales
  4. ¿Cómo crear ficheros Index?
  5. ¿Cómo crear ficheros Release?
  6. ¿Cómo crear Estanques?
    1. Herramientas
  7. ¿Cómo usar los Repositorios?


## Los Repositorios (definición)

Un repositorio es un conjunto de paquetes Debian organizados en un directorio en árbol especial, el cual también contiene unos pocos ficheros adicionales con los índices e información de los paquetes. Si un usuario añade un repositorio a su fichero `sources.list`, él puede ver e instalar facilmente todos los paquetes disponibles en éste al igual que los paquetes contenidos en Debian.

## ¿Cómo funcionan los repositorios?

Un repositorio consiste en al menos un directorio con algunos paquetes `DEB` en él, y dos ficheros especiales que son el `Packages.gz` para los paquetes binarios y el `Sources.gz` para los paquetes de las fuentes. Una vez que tu repositorio esté listado correctamente en el `sources.list`, si los paquetes binarios son listados con la palabra clave `deb` al principio, `apt-get` buscará en el fichero índice `Packages.gz`, y si las fuentes son listadas con las palabras claves `deb-src` al principio, éste buscará en el fichero indice `Sources.gz`. Ésto se debe a que en el fichero `Packages.gz` se encuentra toda la información de todos los paquetes, como nombre, version, tamaño, descripción corta y larga, las dependencias y alguna información adicional que no es de nuestro interés. Toda la información es listada y usada por los _Administradores de Paquetes_ del sistema tales como `dselect` o `aptitude`. Sin embargo, en el fichero `Sources.gz` se encuentran listados todos los nombres, versiones y las dependencias de desarrollo (esto es, los paquetes necesitados para compilar) de todos los paquetes, cuya información es usada por `apt-get source` y herramientas similares.

Una vez que hayas establecido tus repositorios, serás capaz de listar e instalar todos sus paquetes junto a los que vienen en los discos de instalación Debian; una vez que hayas añadido el repositorio deberás ejecutar en la consola:

    $ sudo apt-get update

Ésto es con el fin de actualizar la base de datos de nuestro APT y así el podrá "decirnos" cuales paquetes disponemos con nuestro nuevo repositorio. Los paquetes serán actualizados cuando ejecutemos en consola.

    $ sudo apt-get upgrade

## ¿Cómo establecer Repositorios?

Existen dos tipos de repositorios: los complejos, que es donde el usuario sólo tiene que especificar la ruta base de el repositorio, la distribución y los componentes que él quiera (APT automáticamente buscará los paquetes correctos para la arquitectura correspondiente, si están disponibles), y los más simples, donde el usuario debe especificar la ruta exacta (aqui APT no hará magia para encontrar cuales de los paquetes son los indicados). El primero es  más difícil de establecer, pero es más fácil de utilizar, y siempre debería ser usado para repositorios complejos y/o plataformas cruzadas; el último, es más fácil de establecer, pero sólo debería ser usado para repositorios pequeños o de una sola arquitectura.

Aunque no es realmente correcto, aquí llamaré al primero _Repositorios Automáticos_ y al último _Repositorios Triviales_.

### Repositorios Automáticos

La estructura del directorio de un repositorio automático con las arquitecturas estándares de Debian y sus componentes se asemeja mucho a ésto:

    (tu repositorio root)
    | 
    +-dists
      | 
      |-stable
      | |-main
      | | |-binary-alpha 
      | | |-binary-arm
      | | |-binary-...
      | | +-source 
      | |-contrib
      | | |-binary-alpha 
      | | |-binary-arm
      | | |-binary-...
      | | +-source 
      | +-non-free
      |   |-binary-alpha
      |   |-binary-arm
      |   |-binary-...
      |   +-source
      |
      |-testing 
      | |-main
      | | |-binary-alpha 
      | | |-binary-arm
      | | |-binary-...
      | | +-source 
      | |-contrib
      | | |-binary-alpha 
      | | |-binary-arm
      | | |-binary-...
      | | +-source 
      | +-non-free
      |   |-binary-alpha
      |   |-binary-arm
      |   |-binary-...
      |   +-source
      |
      +-unstable 
        |-main
        | |-binary-alpha 
        | |-binary-arm
        | |-binary-...
        | +-source 
        |-contrib
        | |-binary-alpha 
        | |-binary-arm
        | |-binary-...
        | +-source 
        +-non-free
          |-binary-alpha
          |-binary-arm
          |-binary-...
          +-source

Los paquetes libres van en el directorio `main`; los que no son libres van en el directorio `non-free` y los paquetes libres que dependen de los que no son libres van en el directorio `contrib`.

Existen también otros directorios poco comunes que son el `non-US/main` que contienen paquetes que son libres pero que no pueden ser exportados desde un servidor en los Estados Unidos y el directorio `non-US/non-free` que contiene paquetes que tienen alguna condición de licencia onerosa que restringe su uso o redistribución. No pueden ser exportados de los Estados Unidos porque son paquetes de software de cifrado que no están gestionados por el procedimiento de control de exportación que se usa con los paquetes de `main` o no pueden ser almacenados en servidores en los Estados Unidos por estar sujetos a problemas de patentes.

Actualmente [Debian](http://www.us.debian.org/) soporta **11** tipos de arquitecturas; en éste ejemplo se han omitido la mayoría de ellas por el bien de la brevedad. Cada directorio `binary-*` contiene un fichero `Packages.gz` y un fichero opcional `Release`; cada directorio fuente contiene un fichero `Sources.gz` y también contiene un fichero opcional `Release`.

Nota que los paquetes no tienen que estar en el mismo directorio como los ficheros índices, porque los ficheros índices contienen las rutas a los paquetes individuales; de hecho, podrían estar ubicados en cualquier lugar en el repositorio. Ésto hace posible poder crear estanques.

Somos libres de crear tantas distribuciones como componentes y llamarlos como queramos; las que se usan en el ejemplo son, justamente las usadas en Debian. Podríamos, por ejemplo, crear las distribuciones `current` y `beta` (en vez de `stable`,  `unstable` y `testing`, y que los componentes sean `foo`, `bar`, `baz` y `qux` (en lugar de `main`, `contrib` y `non-free`).

Ya que somos libres de llamar los componentes como queramos, siempre es recomendable usar las distribuciones estándar de Debian, porque son los nombres  que los usuarios de Debian esperan.

### Repositorios Triviales

Los repositorios triviales, consisten en un directorio raíz y tantos sub-directorios
como deseemos. Como los usuarios tienen que especificar la ruta a la raíz del repositorio
y la ruta relativa entre la raíz y el directorio con los ficheros indices en él, somos libres de hacer lo que queramos (inclusive, colocar todo en la raíz del repositorio; entonces, la ruta relativa simplemente sería `/`. Se parecen mucho a ésto:

    (your repository root)
    |
    |-binary
    +-source
    
## ¿Cómo crear ficheros Index?

`dpkg-scanpackages` es la herramienta con la que podemos generar el fichero `Packages` y con la herramienta `dpkg-scansources` creamos los ficheros `Sources`. Ellos pueden enviar sus salidas a `stout`; por consiguiente, para generar ficheros comprimidos, podemos usar una cadena de comandos como ésta:
    
    $ dpkg-scanpackages arguments | gzip -9c > Packages.gz

Las dos herramientas trabajan de la misma manera; ambas toman dos argumentos (en realidad son más, pero aquí no hablaremos de eso; puedes leerte las páginas del manual si quieres saber más); el primer argumento es el directorio en cual están los paquetes, y el segundo es el fichero predominante. En general no necesitamos los ficheros predominantes para repositorios simples, pero como éste es un argumento requerido, simplemente lo pasamos a `/dev/null`. `dpkg-scanpackages` escanea los paquetes `.deb`, sin embargo, `dpkg-scansources` escanea los ficheros `.dsc`, por lo tanto es necesario colocar los ficheros `.orig.gz`, `.diff.gz` y `.dsc` juntos. Los ficheros `.changes` no son necesarios. Así que, si tienes un repositorio trivial como el mostrado anteriormente, puedes crear los dos ficheros indice de la siguiente manera:

    $ cd my-repository
    $ dpkg-scanpackages binary /dev/null | gzip -9c > binary/Packages.gz
    $ dpkg-scansources source /dev/null | gzip -9c > source/Sources.gz

Ahora bien, si tienes un repositorio tan complejo como el mostrado en el primer ejemplo, tendrás que escribir algunos scripts para automatizar éste proceso. También puedes usar el argumento `pathprefix` de las dos herramientas para simplificar un poco la sintaxis.

## ¿Cómo crear ficheros Release?

Si quieres permitirle a los usuarios de tu repositorio usar el `pinning` con tu repositorio, entonces deberás incluir un fichero `Release` en cada directorio que contenga un fichero `Index`. (Puedes leer más acerca del `pinning` en el COMO APT). Los ficheros `Release` son ficheros de texto simple y cortos que tienen una forma muy parecida a la que sigue:

    Archive: archivo
    Component: componente
    Origin: TuCompañia
    Label: TuCompañia Debian repositorio
    Architecture: arquitectura

 * Archive: El nombre de la distribución de Debian. Los paquetes en éste directorio pertenecen a (o estan diseñados para), por ejemplo, `stable`, `testing` o `unstable`.
 * Component: Aquí van los componentes de los paquetes en el directorio, por ejemplo, `main`, `non-free` o `contrib`.
 * Origin: El nombre de la persona que hizo los paquetes.
 * Label: Algunas etiquetas adecuadas para los paquetes de tu repositorio. Usa tu imaginación.
 * Architecture: La arquitectura de lo paquetes en éste directorio, como `i386` por ejemplo, `sparc` o fuente. Es importante que se establezcan `Architecture` y `Archive` de manera correcta, ya que ellos son más usados para hacer `pinning`. Los otros, sin embargo, son menos importantes en éste aspecto.

## ¿Cómo crear estanques?

Con los repositorios automáticos, distribuir los paquetes en los diferentes directorios puede tornarse rápidamente en una bestia indomable, además también se gasta mucho espacio y ancho de banda, y hay demasiados paquetes (como los de la documentación, por ejemplo) los cuales son los mismos para todas las arquitecturas.

En éstos casos, una posible solución es un estanque. Un estanque es un directorio adicional dentro del directorio raíz del repositorio, que contiene todos los paquetes (los binarios para todas las arquitecturas, distribuciones y componente y todas las fuentes). Se pueden evitar muchos problemas, a través de una combinación inteligente de ficheros predominantes (tema que no se toca en éste documento) y de _scripts_. Un buen ejemplo de un reposotorio "estancado" es el propio repositorio de Debian.

Los estanques sólo son útiles para repositorio grandes. Nunca he hecho uno y no creo que lo haga en un futuro cercano y ésa es la razón por la cual no se explica como hacerlo aquí. Si tu crees que esa sección debería ser añadida siéntete libre de escribir una y contáctame luego.

### Herramientas

Existen varias herramientas para automatizar y facilitar la creación de ficheros Debian. A continuación son listados los más importantes:

 * `apt-ftparchive`: Es la línea de comandos de la herramienta usada para generar los ficheros indice que APT utiliza para accesar a la fuente de una distribución.
 * `apt-move`: Es usado para mover una colección de ficheros paquetes de Debian a un fichero jerárquico como el usado en el fichero oficial Debian. Éste es parte del paquete `apt-utils`.

## ¿Cómo usar los repositorios?

Usar un repositorio es muy sencillo, pero ésto depende de el tipo de repositorio que hayas creado, ya sea binario o de fuentes, automático o trivial. Cada repositorio ocupa una línea en el fichero `sources.list`. Para usar un repositorio binario solo tenemos que usar `deb` al principio de la línea y para usar un repositorio de
fuentes, en vez de `deb`, sólo tenemos que agregrar `deb-src`. Cada línea tiene la siguiente sintaxis:

    deb|deb-src uri distribución [componente1] [componente2] [...]

El `URI` es el Identificador Universal de Recursos de la raíz del repositorio, como por ejemplo: `ftp://ftp.tusitio.com/debian`, `http://tusitio.com/debian`, o, para ficheros locales, `file::///home/joe/mi-repositorio-debian/`. Donde la barra inclinada es opcional. Para repositorios automáticos, tienes que especificar la distribución y uno o más componentes; la distribución no debe terminar con una inclinada.

A continuación unos ejemplos de repositorios:

    deb ftp://sunsite.cnlab-switch.ch/mirror/debian/ unstable main contrib non-free
    deb-src ftp://sunsite.cnlab-switch.ch/mirror/debian/ unstable main contrib non-free
    deb file:///home/aisotton/rep-exact binary/
    deb-src file:///home/aisotton/rep-exact source/

Donde los dos primeros se corresponden con repositorios de tipo Automático y los dos últimos Triviales.

Lista de paquetes en la [distribución estable](http://packages.debian.org/stable/) de Debian.
Lista de paquetes en la [distribución testing](http://packages.debian.org/testing/) de Debian
Lista de paquetes en la [distribución inestable](http://www.debian.org/Packages/unstable/) de Debian

**Artículo original** [Debian Repository HOWTO](http://www.debian.org/doc/manuals/repository-howto/repository-howto.en.html) por [Aaron Isotton](mailto:aaron@isotton.com)
