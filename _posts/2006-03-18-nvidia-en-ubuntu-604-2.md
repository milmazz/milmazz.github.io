---
title: nVIDIA en Ubuntu 6.04
author: chantanito
date: 2006-03-18 11:10:17
categories:
  - GNU/Linux
  - Ubuntu
tags:
  - dapper
  - GNU/Linux
  - nvidia
  - Ubuntu
slug: nvidia-en-ubuntu-604-2
redirect_from: /archivos/2006/03/18/nvidia-en-ubuntu-604-2/
---

Hace unos días actualicé mi [Ubuntu 5.10](http://www.ubuntu.com/news/release510) a una de sus últimas  versiones de _testing_: [Flight 5](http://www.ubuntu.com/testing/flight5). Debo admitir que quedé anonadado por lo cambios en la distribución, ya que con todas las mejoras que tiene parece que hubiera pasado, no sólo 6 meses sino años. El [Dapper Drake](http://cdimage.ubuntu.com/releases/dapper/flight-5/) (Flight 5, Ubuntu 6.04) es mucho mejor que sus antecesores. Pero la razón por la cual me  decidí a escribir éste artículo es otra: **la instalación de los drivers de mi tarjeta nVIDIA** y su puesta en funcionamiento a punto.

Cuando actualicé mi sistema no hubo ningún problema, y lo digo en serio, ningún problema de ninguna índole. Toda mi configuración de escritorio quedó intacta; pero empecé a notar que la configuración de la tarjeta de video no se cargaba con el sistema. Entonces supe que, por alguna razón, los drivers de la tarjeta habían cambiado, es decir, el sistema asignó el driver por defecto para la tarjeta, más no los drivers de la tarjeta misma. Entonces tuve que ponerme a configurar la tarjeta.

Lo primero que hice fué verificar que los drivers vinieran con la distribución. Lo hice con la siguiente línea:

    $ sudo aptitude search nvidia

Con lo cual obtuve lo siguiente:

    i  nvidia-glx                      - NVIDIA binary XFree86 4.x/X.Org driver
    v  nvidia-kernel-1.0.7174          -
    v  nvidia-kernel-1.0.8178          -
    i  nvidia-kernel-common            - NVIDIA binary kernel module common files

Entonces ya sabía que los drivers venían con la distro, lo cual me pareció fascinante, ya que en realidad el Flight 5, no es la versión definitiva del Dapper Drake. Luego procedí a verificar la documentación de dicho paquete. Ésto lo hice con la siguiente línea de comandos:

    $ sudo aptitude show nvidia-glx

Esto lo hice para verificar que no haya alguna clase de conflictos con otros paquetes, pero en realidad no es un paso necesario, ya que `aptitude` resuelve todo tipo de conflictos y dependencias. Después de verificar que todo estaba en orden me decidí a instalar los drivers. Ésto lo hice con la siguiente linea de comandos:

    $ sudo aptitude install nvidia-glx

Con lo cual quedaron instalados los drivers de la tarjeta de manera trasparente y rápida. Lo siguiente que debía hacer, era activar la configuración de la tarjeta. Lo cual hice con la siguiente línea de comandos:

    $ sudo nvidia-glx-config enable

Una vez hecho ésto ya podía configurar la tarjeta. Algo que hay que hacer notar es que, para las distribuciones anteriores de [Ubuntu](http://www.ubuntu.com), había que instalar de manera separada el paquete `nvidia-glx` y el `nvidia-settings`, sin embargo, aquí queda todo instalado de una vez. Lo que sigue es iniciar la configuración de la tarjeta, lo cual hice con la siguiente línea de comandos:

    $ nvidia-settings

Y ya tenía acceso a la configuración de mi tarjeta. Sin embargo, al hacer todo ésto, la configuración no se carga al iniciar el sistema, pero no fué problema, porque lo solucioné colocando en los programas de inicio del `gnome-session-manager` los siguiente:

    nvidia-settings -l

Este comando carga la configuración de `nvidia-settings` que tengamos actualmente. Es lo mismo que, una vez que haya cargado el sistema, ejecutemos en la consola éste comando, sólo que ahora se va a ejecutar apenas inicie el sistema operativo.

## Otros ajustes...

Si quieren colocar un lanzador en los menús del panel de gnome deben hacer los siguiente:

    $ sudo gedit /usr/share/applications/NVIDIA-Settings.desktop

Y luego insertar lo siguiente en dicho fichero:

    [Desktop Entry]
    Name=Configuración nVIDIA
    Comment=Abre la configuración de nVIDIA
    Exec=nvidia-settings
    Icon=(el icono que les guste)
    Terminal=false
    Type=Application
    Categories=Application;System;

Y ya tendrán un lanzador en los menús del panel de gnome. Una opción sería utilizar el editor de menús  Alacarte.

### `nvidia-xconf`

`nvidia-xconf` es una utilidad diseñada para hacer fácil la edición de la configuración de X. Para ejecutarlo simplemente debemos escribir en nuestra consola lo siguiente:

    $ sudo nvidia-xconfig

**Pero en realidad, ¿qué hace `nvidia-xconfig`?** `nvidia-xconfig`, encontrará el fichero de configuración de X y lo modificará para usar el driver nVIDIA X. Cada vez que se necesite reconfigurar el servidor X se puede ejecutar desde la terminal. Algo interesante es que cada vez que modifiquemos el fichero de configuración de X con `nvidia-xconfig`, éste hará una copia de respaldo del fichero y nos mostrará el nombre de dicha copia. Algo muy parecido a lo que sucede cada vez que hacemos:

    dpkg-reconfigure xserver-xorg

Una opción muy útil de `nvidia-xconfig` es que podemos añadir resoluciones al fichero de configuración de X simplemente haciendo:

    $ sudo nvidia-xconfig --mode=1280x1024

...por ejemplo.
