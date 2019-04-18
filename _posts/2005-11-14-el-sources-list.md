---
title: El fichero sources.list
author: chantanito
date: 2005-11-14 14:15:54
categories:
  - debian
  - Ubuntu
tags:
  - debian
  - Ubuntu
slug: el-sources-list
redirect_from: /archivos/2005/11/14/el-sources-list/
---

La mayoría de los entusiastas de sistemas Linux, tarde o temprano llegan a toparse con ésta interrogante. En una forma bastante general, podríamos definir a éste fichero como la lista de recursos de paquetes que es usada para localizar los ficheros del sistema de distribución de paquetes usado en el sistema. Este fichero de control está ubicado en la carpeta `/etc/apt/` de nuestro sistema. El fichero es un simple documento de texto sencillo que puede ser modificado con cualquier editor de textos.

Dentro de éste fichero nos vamos a encontrar una serie de líneas, que no son más que las procedencias de los recursos ubicados en los repositorios que elijamos. Éstas líneas de procedencias tienen una forma general que es: `tipo`, `uri`, `distribución` y `complementos`.

Entonces, las formas generales de las líneas de procedencias sería así:

    deb uri distribución [componente1] [componente2] [...]
    deb-src uri distribución [componente1] [componente2] [...]

## ¿Qué debo saber sobre el `sources.list`?

Debemos tener en cuenta varios aspectos sobre éste fichero tan importante. Por ejemplo, hay algo que muchos no saben e ignoran, y es que ésta lista de procedencias está diseñada para soportar cualquier número y distintos tipos de procedencias, por supuesto, la demora del proceso de actualización de la base de datos del APT va a ser proporcional al número de procedencias, ya que mientras más procedencias, mayor es la cantidad de paquetes a añadir a la base de datos, y también va a durar un poco más de tiempo, dependiendo de nuestra velocidad de conexión.

El fichero lista una procedencia por línea, con la procedencia de mayor prioridad en la primera línea, como por ejemplo, cuando tenemos los paquetes en discos `CD-ROM`, entonces ubicamos éste de primero. Como ya mencioné, el formato de cada línea es:

    tipo  uri  distribución complementos

Donde:

 * `tipo`: Determina el formato de los argumentos, que pueden ser de dos tipos: `deb` y `deb-src`. El tipo `deb` hace referencia a un típico archivo de Debian de dos  niveles, que son distribución y componente, sin embargo, el  tipo  `deb-src`  hace referencia al código fuente de la distribución y tiene la misma sintaxis que las de tipo `deb`. Las líneas de tipo `deb-src` son necesarias si queremos descargar un índice de los paquetes que tienen el código fuente disponible, entonces de ésta forma obtendremos los códigos originales, más un fichero de control, con extensión `.dsc` y un fichero adicional `diff.gz`, que contiene los cambios necesario para debianizar el código.
 * `uri`: Identificador Universal de Recursos, ésto es, el tipo de recurso de la cual estamos obteniendo nuestros paquetes. Pero ¿Cuáles son los tipos de `uri` que admite nuestra lista de procedencias? A continuación hago mención
de las más populares, por así decirlo:
 	* `CD-ROM`: El cdrom permite a APT usar la unidad de CD-ROM  local. Se puede usar el programa `apt-cdrom` para  añadir  entradas  de  un  cdrom  al fichero `sources.list` de manera automática, en modo consola.
	* `FTP`: Especifica un servidor FTP como archivo.
 	* `HTTP`: Especifica un servidor HTTP como archivo.
	* `FILE`: Permite considerar como archivo a cualquier fichero en el sistema  de  ficheros.  Esto  es  útil para particiones montadas mediante NFS (sistema de ficheros usado para montar particiones de sistemas remotos) y réplicas locales.
* `distribución`: Aquí especificamos la distribución en la cual estamos trabajando, bien sea [Debian](http://www.debian.org), [Ubuntu](http://www.ubuntu.com), [Kubuntu](http://www.kubuntu.org), [Gnoppix](http://www.gnoppix.org),[Knoppix](http://www.knoppix.org) y otras, basadas en sistemas Debian GNU/Linux. `distribución` también puede contener una variable, `$(ARCH)`, que se expandirá en la arquitectura de Debian usada en el  sistema  (`i386`,  `m68k`,  `powerpc`,...). Esto  permite que `sources.list` no sea dependiente de la arquitectura del sistema.
 * `componentes`: Los componentes son los tipos de repositorios clasificados **según las licencias de los paquetes que contienen**. Dentro de los	componentes tenemos `main`, `contrib` y `non-free`, para usuarios Debian; sin embargo para usuarios Ubuntu, por ejemplo, también existen `universe`, `multiverse` `restricted`. Ahora la decisión de cuales repositorios utilizar, eso va más allá de lo pueda ser explicado acá, ya que eso le concierne a su persona.

Entonces, la forma de una línea de procedencias quedaría algo así:

    # deb http://security.ubuntu.com/ubuntu breezy-security main restricted
    # deb-src http://security.ubuntu.com/ubuntu breezy-security main restricted

Ahora bien, se preguntarán ¿Por qué el carácter `#` (almohadilla) al principio de la línea? Bueno, la respuesta es muy simple. Éste caracter se utiliza para indicarle al APT cuando ignorar, por así decirlo, las líneas que contengan dicho caracter al principio, pues lo que hace en realidad es tomarlas como comentarios de lenguaje y simplemente no las interpreta, por lo tanto, si queremos que el APT tome o no en cuenta una línea de procedencias, entonces quitamos o añadimos el caracter, respectivamente.

**Nota del autor**: Algunas partes de este artículo fueron tomadas del manual de Debian.
