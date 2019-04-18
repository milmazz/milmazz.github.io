---
title: Comprobando las imágenes de ubuntu
author: milmazz
date: 2005-10-14 22:03:26
categories:
  - Ubuntu
tags:
  - Ubuntu
slug: comprobando-las-imagenes-de-ubuntu
redirect_from: /archivos/2005/10/14/comprobando-las-imagenes-de-ubuntu/
---

Si usted es de las personas que ha descargado la última versión de
[Ubuntu](http://www.ubuntulinux.org), para el momento en el que se redactó esta
entrada es [Breezy Badger](http://www.ubuntu.com/newsitems/release510), es
importante que verifique la autenticidad de la imagen que posee, para ello
comprobaremos las sumas de control MD5.

En primer lugar, debe poseer un fichero que posea las sumas de control MD5 de la
distribución, normalmente desde el sitio donde descarga las distintas versiones
se ubuntu se dispone de uno, en el caso de la versión _Breezy Badger_ disponemos
del fichero [MD5SUMS](http://releases.ubuntu.com/5.10/MD5SUMS), la cual puede
encontrar en el sitio de [descargas de Breezy
Badger.](http://releases.ubuntu.com/5.10/)

Una vez descargado debemos compararlo con las sumas de control MD5 generadas
para la imagen que poseemos de la versión en nuestro medio de almacenamiento,
p.ej. disco duro.

En el ejemplo que presentaré a continuación el fichero que posee las sumas de
control MD5 que he descargado desde el sitio oficial de ubuntu se encuentra en
el directorio `/mnt/backup/distros/`, en este mismo directorio tengo la imagen
de ubuntu, `ubuntu-5.10-install-i386.iso`, en resumidas cuentas, el comando a
utilizar para la comprobación de las sumas MD5 es el `md5sum`, el modo de uso es
el siguiente.

    milmazz@omega:/mnt/backup/distros$ <kbd>md5sum -cv MD5SUMS</kbd>

En este caso, la opción `-c` nos permite comprobar la suma de control MD5 para
todos los archivos listados en el fichero `MD5SUMS`, la opción `-v` nos permite
obtener más detalle de la operación, por ejemplo:

    ubuntu-5.10-install-i386.iso Correcto

El fichero `MD5SUMS` posee **todas** las sumas de control MD5 de las distintas
imágenes que ofrece ubuntu, tanto las imágenes que poseen las versiones
_instalables_ como los LiveCD, en las distintas arquitecturas. Si lo desea,
puede editar el fichero, simplemente haciendo uso de `vi`, por ejemplo:

    $ vi MD5SUMS

Una vez dentro de `vi`, debe eliminar las lineas que no desee, para ello
simplemente haga uso de `dd`, una vez eliminadas todas las entradas que no
desee, guarde los cambios, para ello, presione la tecla Esc y seguidamente
escriba :wq y presione la tecla Enter.
