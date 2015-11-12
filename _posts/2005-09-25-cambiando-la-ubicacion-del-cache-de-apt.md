---
redirect_from: "/archivos/2005/09/25/cambiando-la-ubicacion-del-cache-de-apt/"
author: milmazz
comments: true
date: 2005-09-25 05:12:58
layout: post
slug: cambiando-la-ubicacion-del-cache-de-apt
title: Cambiando la ubicación del cache de apt
wordpress_id: 85
categories:
- GNU/Linux
- Ubuntu
tags:
- GNU/Linux
- Ubuntu
---

El día 21 de Septiembre _Jorge Villarreal_ preguntó en la entrada [Creando un repositorio local](/archivos/2005/07/24/creando-un-repositorio-local/) lo siguiente:

> ... tengo un laptop lentisimo , ahora mi pregunta ya que puedo navegar desde la oficina y en ella solo tienen windows como hago para descargar actualizaciones y llevarlas en una pen o en cd?. a como anexo en las tardes navego con cd-live ubuntu, me pregunto si podre bajar virtualmente desde ahi?

Mi respuesta es la siguiente:

Sí, en efecto puedes descargar los paquetes desde tu LiveCD de [Ubuntu](http://ubuntulinux.org/), de hecho, existen dos maneras para mí, las expongo a continuación.

## Primer método

La primera es usar la memoria que dispones, la cual es limitada, recuerda que los ficheros que descarga la interfaz `apt` (o `aptitude`) se almacenan en el directorio /var/cache/apt/archives, como te mencione anteriormente, este método puede ser limitado.

Veamos ahora el segundo método, te recomiendo éste porque vamos a escribir en el disco duro.

## Segundo método

Ya que en la oficina utilizas Windows, el único requisito que se necesita es disponer de una partición cuyo formato de ficheros sea FAT, asumiré en el resto de mi respuesta que dicha partición se encuentra en /dev/hdb1 y se ha montado en /mnt/backup. Por lo tanto:

    $ sudo mount -t vfat /dev/hdb1 /mnt/backup

Posteriormente se debe crear el fichero /etc/apt.conf, esto se puede hacer fácilmente con cualquier editor. Dicho fichero debe contener lo siguiente:

    DIR "/"
    {
      Cache "mnt/backup/apt/" {
        Archives "archives/";
        srcpkgcache "srcpkgcache.bin";
        pkgcache "pkgcache.bin";
      };
    };

Lo anterior simplemente está cambiando el directorio usual (/var/cache/apt/archives) del _cache_, de ahora en adelante se estará escribiendo de manera permanente en disco duro. Previamente debes haber creado el directorio /mnt/backup/apt/archives/. Seguidamente tienes que crear el fichero `lock` y el directorio `partial`. Resumiendo tenemos:

    $ mkdir -p /mnt/backup/apt/archives/partial
    $ touch /mnt/backup/apt/archives/lock

## Pasos comunes en ambos métodos

Recuerda que sea cual sea el método que decidas usar, debes editar el fichero /etc/apt/sources.list, mejora la lista de repositorios que se presentan, luego de guardar los cambios en el fichero, ejecuta el siguiente comando.

    $ sudo aptitude update

El comando anterior actualizará tú lista de paquetes con los que se encuentran en los repositorios que añadiste previamente. Ahora bien, para almacenar ficheros en el directorio _cache_ haz uso del comando.

    $ sudo aptitude --download-only install <em>packages</em>

Cuando me refiero a _packages_ recuerda que son los nombres de los paquetes.

Seguidamente puedes seguir los pasos que se te indican en la entrada [Creando un repositorio local](/archivos/2005/07/24/creando-un-repositorio-local/), por supuesto, si cambias la dirección del _cache_ que hará uso la interfaz `apt` (o `aptitude`) debes hacer los ajustes necesarios. Espero te sirva la información.

Si alguien desea realizar un aporte bienvenido será.