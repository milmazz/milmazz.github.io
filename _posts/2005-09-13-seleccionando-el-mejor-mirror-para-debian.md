---
redirect_from: "/archivos/2005/09/13/seleccionando-el-mejor-mirror-para-debian/"
author: milmazz
comments: true
date: 2005-09-13 12:48:12
layout: post
slug: seleccionando-el-mejor-mirror-para-debian
title: Seleccionando el mejor mirror para debian
categories:
- debian
- GNU/Linux
tags:
- debian
- GNU/Linux
---

El día de ayer decidí instalar [Debian](http://www.debian.org/)
[Sarge](http://www.debian.org/releases/sarge/) en uno de los ordenadores de
casa, la instalación base de maravilla, luego procedi a levantar el entorno
gráfico de [GNOME](http://www.gnome.org/) haciendo uso de
[aptitude](/article/2005/07/28/aptitude-%c2%bfaun-no-lo-usas/), deje de lado
muchas aplicaciones que no voy utilizar extensivamente. Mientras intento
solucionar un _problemita_ con el sonido me dispuse a indagar acerca de los
repositorios que ofrece Debian.

Leyendo la lista de _[mirrors](http://www.debian.org/mirror/list)_ en el sitio
oficial de Debian se me ocurrio que debia existir una manera de medir la rapidez
de cada uno de ellos, quizá para muchos  esto no es nuevo, para mí si lo es,
recien comienzo con esta
[distro](http://es.wikipedia.org/wiki/Distribuciones_de_Linux), aunque aún
mantengo [Ubuntu](http://ubuntulinux.org/) (no se preocupen mis dos o tres
lectores que seguiré escribiendo acerca de esta excelente distro). Bueno, he
hecho uso de `apt-spy`, este paquete hace una serie de pruebas sobre los mirrors
de debian, midiendo la su ancho de banda y su latencia.

El paquete `apt-spy` por defecto reescribe el fichero `/etc/apt/sources.list`
con los servidores con los resultados más rápidos.

Para instalarlo simplemente hacer lo siguiente:

    # aptitude install apt-spy

Leyendo el manual de esta aplicación se puede observar que existe la opción de
seleccionar a cuales mirrors se les harán las pruebas de
acuerdo a su localización geográfica.

Por ejemplo:

    # apt-spy -d stable -a South-America -o mirror.txt

Lo anterior genera un fichero fichero, cuyo nombre será `mirror.txt`, la opción
`-a` indica un área, esta opción acepta los valores siguientes: `Africa`,
`Asia`, `Europe`, `North-America`, `Oceania` y `South-America`, aunque es
posible definir sus propias áreas. La opción `-d` indica la distribución, esta
opcion acepta los valores siguiente: `stable`, `testing` o `unstable`.

He obtenido como resultado lo siguiente:

    milmazz@nautilus:~$ cat mirror.txt
    deb http://ftp.br.debian.org/debian/ stable main
    deb-src http://ftp.br.debian.org/debian/ stable main
    deb http://security.debian.org/ stable/updates main

También he realizado una segunda prueba.

    # apt-spy -d stable -e 10 -o mirror.txt

Obteniendo como respuesta lo siguiente:

    milmazz@nautilus:~$ cat mirror.txt
    deb http://ftp.tu-graz.ac.at/mirror/debian/ stable main
    deb-src http://ftp.tu-graz.ac.at/mirror/debian/ stable main
    deb http://security.debian.org/ stable/updates main

La opción `-e` es para detener el análisis después de haber completado 10 (o el
número entero indicado como parámetro en dicha opción) servidores.

Me he quedado con los mirrors de Brazil (los mostrados en la primera prueba) por
su cercanía geográfica, los del segundo análisis resultan ser de Austria y
entran en la categoría de mirrors secundarios.
