---
redirect_from: "/archivos/2007/02/18/grub-mejorando-nuestro-gestor-de-arranque/"
author: milmazz
comments: true
date: 2007-02-18 00:22:38
layout: post
slug: grub-mejorando-nuestro-gestor-de-arranque
title: 'GRUB: Mejorando nuestro gestor de arranque'
categories:
- debian
- GIMP
- Seguridad
tags:
- debian
- GIMP
- grub
- Seguridad
---

Anteriormente había comentado en la primera entrega del artículo [Debian: Bienvenido al Sistema Operativo Universal](/article/2006/04/25/debian-bienvenido-al-sistema-operativo-universal-parte-i/) que por medidas de seguridad establezco las opciones de montaje `ro`, `nodev`, `nosuid`, `noexec` en la partición `/boot`, donde se encuentran los ficheros estáticos del gestor de arranque.

El gestor de arranque que manejo es GRUB. Por lo tanto, el motivo de este artículo es explicar como suelo personalizarlo, tanto para dotarle de seguridad como mejorar su presentación.

## Seguridad

Lo primero que hago es verificar el dueño y los permisos que posee el fichero `/boot/grub/menu.lst`, en mi opinión la permisología más abierta y **peligrosa** sería **644**, pero normalmente la establezco en **600**, evitando de ese modo que todos los usuarios (excepto el dueño del fichero, que en este caso será `root`) puedan leer y escribir en dicho fichero. Para lograr esto recurrimos al comando `chmod`.

    # chmod 600 /boot/grub/menu.lst

Si usted al igual que yo mantiene a `/boot` como una partición de solo lectura, deberá montar de nuevo la partición `/boot` estableciendo la opción de escritura, para lo cual hacemos:

    # mount -o remount,rw /boot

Después de ello si podrá cambiar la permisología del fichero `/boot/grub/menu.lst` de ser necesario.

El segundo paso es evitar que se modifique de manera interactiva las opciones de inicio del _kernel_ desde el gestor de arranque, para ello estableceremos una contraseña para poder editar dichas opciones, pero primero nos aseguraremos de cifrar esa contraseña con el algoritmo MD5. Por lo tanto, haremos uso de `grub-md5-crypt`

    # grub-md5-crypt
    Password:
    Retype password:
    $1$56z5r1$yMeSchRfnxdS3QDzLpovV1

La última línea es el resultado de aplicarle el algoritmo MD5 a nuestra contraseña, la copiaremos e inmediatamente procedemos a modificar de nuevo el fichero `/boot/grub/menu.lst`, el cual debería quedar más o menos como se muestra a continuación.

    password --md5 $1$56z5r1$yMeSchRfnxdS3QDzLpovV1

    title           Debian GNU/Linux, kernel 2.6.18-3-686
    root            (hd0,1)
    kernel          /vmlinuz-2.6.18-3-686 root=/dev/sda1 ro
    initrd          /initrd.img-2.6.18-3-686
    savedefault

    title           Debian GNU/Linux, kernel 2.6.18-3-686 (single-user mode)
    root            (hd0,1)
    kernel          /vmlinuz-2.6.18-3-686 root=/dev/sda1 ro single
    initrd          /initrd.img-2.6.18-3-686
    savedefault

La instrucción `password --md5` aplica a nivel global, así que cada vez que desee editar las opciones de inicio del _kernel_, tendrá que introducir la clave (deberá presionar la tecla p para que la clave le sea solicitada) que le permitirá editar dichas opciones.

## Presentación del gestor de arranque

A muchos quizá no les agrade el aspecto inicial que posee el GRUB, una manera de personalizar la presentación de nuestro gestor de arranque puede ser la descrita en la segunda entrega de la entrada [Debian: Bienvenido al Sistema Operativo Universal](/article/2006/05/02/debian-bienvenido-al-sistema-operativo-universal-parte-ii/) en donde instalaba el paquete `grub-splashimages` y posteriormente establecía un enlace simbólico, el problema de esto es que estamos limitados a las imágenes que trae solo ese paquete.

A menos que a usted le guste diseñar sus propios fondos, puede usar los siguientes recursos:

  * [Mcgrof's collection of splashimages](http://ruslug.rutgers.edu/~mcgrof/grub-images/images/)
  * [GRUB splash images](http://schragehome.de/splash/)
  * [Grub SplashImages from schultz-net](http://www.schultz-net.dk/grub.html)

Después de escoger la imagen que servirá de fondo para el gestor de arranque, la descargamos y la colocamos dentro del directorio `/boot/grub/`, una vez allí procedemos a modificar el fichero /boot/grub/menu.lst y colocaremos lo siguiente:

    splashimage=(hd0,1)/grub/debian.xpm.gz

En la línea anterior he asumido lo siguiente:

  * La imagen que he colocado dentro del directorio `/boot/grub/` lleva por nombre `debian.xpm.gz`
  * He ajustado la ubicación de mi partición `/boot`, es probable que en su caso sea diferente, para obtener dicha información puede hacerlo al leer la tabla de particiones con `fdisk -l` o haciendo uso del comando `mount`.

    $ mount | grep /boot
    /dev/sda2 on /boot type ext2 (rw,noexec,nosuid,nodev)
    # fdisk -l | grep ^/dev/sda2
    /dev/sda2            1217        1226       80325   83  Linux

Por lo tanto, la ubicación de la partición `/boot` es en el primer disco duro, en la segunda partición, recordando que la notación en el grub comienza a partir de **cero** y _no_ a partir de uno, tenemos como resultado `hd0,1`.

También puede darse el caso que ninguno de los fondos para el gestor de arranque mostrados en los recursos señalados previamente sean de su agrado. En ese caso, puede que le sirva el siguiente video demostrativo sobre [como convertir un fondo de escritorio en un Grub Splash Image](http://files.milmazz.com/grub-splashimages.ogg) (2 MB) haciendo uso de [The Gimp](http://www.gimp.org/), espero le sea útil.

Después de personalizar el fichero `/boot/grub/menu.lst` recuerde ejecutar el comando `update-grub` como superusuario para actualizar las opciones.
