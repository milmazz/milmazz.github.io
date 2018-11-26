---
author: milmazz
comments: true
date: 2005-08-22 21:02:34
layout: post
slug: ubuntu-lightweight
title: Ubuntu lightweight
categories:
- Ubuntu
tags:
- Ubuntu
---

Si usted es de esas personas que le gusta obtener el mayor rendimiento, con la
menor carga de procesador posible, a pesar de no contar con hardware de última
generación. Seguramente este artículo le interesará puesto que trataré de
explicarle detalladamente la instalación del entorno de escritorio XFCE,
partiendo de una instalación `server`, la cual instala el sistema base
únicamente, sin entorno gráfico, todo esto por supuesto implementado en Ubuntu
Linux.

Existen 4 opciones para instalar Ubuntu, son las siguientes:

* `linux:` Esta es la opción de instalación por defecto.
* `expert:` Inicia la instalación en modo _experto_, ofrece mayor control sobre
  la instalación.
* `server`, `server-expert:` Ofrece una instalación mínima del sistema, este
  tipo de instalación es ideal para servidores, en donde regularmente el
  administrador instalará el software que realmente necesita, bajo esta opción
  de instalación no se instalará entorno gráfico.

La información anterior la puede verificar al pulsar la tecla F1 en la pantalla
de presentación que aparece al iniciar el sistema con el CD de instalación de
Ubuntu Linux, seguidamente deberá presionar la tecla F3 para consultar dichas
opciones, la tecla F3 se refiere a _methods for special ways of using this
CD-ROM_.

La opción que eligiremos será `server`, con ello instalaremos el sistema base y
posteriormente nos dedicaremos a _levantar_ el entorno gráfico XFCE.

No se entrará en mucho detalle acerca de la instalación en modo `server` porque
el asistente nos va guiando de manera muy intuitiva, quizás la parte más
_díficil_ (y no lo es en lo absoluto) sea el _particionamiento del disco_,
acerca de este último punto tampoco entrare en detalle puesto que en la red
existen infinidad de documentos que hacen referencia a métodos de
particionamiento, yo prefiero hacerlo manualmente, quizás a otros no les guste
el método anterior, pero prefiero tener el control sobre lo que uso. También
existe la posibilidad de realizar un particionamiento automático, asi que no hay
que asustarse.

Una vez finalizada la instalación del sistema base de ubuntu procedemos de la
siguiente manera.

### Estableciendo los repositorios

En primer lugar, vamos a activar y añadir algunos repositorios, para ello
necesitamos editar el fichero `/etc/apt/sources.list`, en mi caso he activado
todos los fuentes `deb` que vienen por defecto en el fichero, por ahora dejo
comentado los `deb-src`. De manera adicional he agregado a la lista un nuevo
repositorio.

    <code>deb http://www.os-works.com/debian testing main</code>

Una vez que haya guardado los cambios en el fichero `/etc/apt/sources.list`
recuerde que debe autenticar el origen de los ficheros binarios o fuentes de
manera transparente, esto se explico con anterioridad en el artículo [COMO
actualizar de manera segura su
sistema](/article/2005/08/08/como-actualizar-de-manera-segura-su-sistema/), el
ejemplo que se expone en dicho artículo aplica perfectamente para los
repositorios de [www.os-works.com](http://www.os-works.com).

Puede bajar una muestra de mi
[sources.list](http://blog.milmazz.com.ve/wp-content/sources.list) si lo
prefiere.

Puede que usted no esté de acuerdo en utilizar el repositorio del grupo
[os-cillation](http://www.os-cillation.com/), yo voy a exponer las razones por
las cuales he decidido elegir dicho repositorio, el equipo _os-cillation_
mantiene de manera _no oficial_ paquetes para el entorno de escritorio
[Xfce](http://www.xfce.org/), estos paquetes son constantemente actualizados, de
hecho, estos paquetes compilados son usados para la creación de la distribución
[Xfld Live-Linux](http://www.xfld.org/). Estos paquetes binarios son para la
arquitectura i386, compilados en una maquina con
[Debian](http://www.debian.org/)
[testing](http://www.nl.debian.org/releases/sarge/) (sarge).

### Estableciendo las preferencias para los repositorios

Seguidamente crearemos el fichero `/etc/apt/preferences`, recuerde que el
fichero lo puede crear haciendo uso del editor `nano` o `vi`, lo importante es
que el fichero contenga lo siguiente:

    <code>Package: *
    Pin: origin www.os-works.com
    Pin-Priority: 999</code>

Después de guardar los cambios del fichero `/etc/apt/preferences` actualice la
lista de paquetes y la distribución.

### Actualizando el sistema actual

    <code>$ sudo aptitude update
    $ sudo aptitude dist-upgrade</code>

Este proceso puede requerir de cierto tiempo, dependiendo de su ancho de banda.

### Instalando xfce, el _display manager_ y los componentes básicos del sistema X Window

Teniendo actualizada la distribución proceda a instalar los siguientes paquetes,
al igual que el paso anterior, el tiempo de espera dependera de su ancho de
banda.

    <code>$ sudo aptitude install x-window-system-core xdm xfld-desktop</code>

Es importante recalcar que le paquete `xfld-desktop` instalará el entorno de
escritorio completo, incluyendo el [emulador del
terminal](http://terminal.os-cillation.com/), el manejador de ficheros
[ROX](http://rox.sf.net/), el reproductor
[Xfmedia](http://spuriousinterrupt.org/projects/xfmedia/) y otros _plugins_ para
el panel. En el caso en que usted desea solo instalar los componentes básicos
del entorno de escritorio Xfce, el paquete a instalar debe ser `xfce4`. Asi que
sustiyendo el paquete anterior, el comando quedaría de la siguiente forma.

    <code>$ sudo aptitude install x-window-system-core xdm xfce4</code>

**Observación:** Este paquete (`xfce4`) también se encuentra disponible en la
sección `universe` de los repositorios de ubuntu, en este último caso no es
necesario hacer uso de los repositorios de _os-works_, aunque como mencione
anteriormente, los repositorios de _os-works_ pueden ofrecerle un paquete más
actualizado.

### Iniciando Xfce

Finalmente en su directorio personal cree un fichero oculto `.xsession`.

    <code>$ nano ~/.xsession</code>

El fichero `.xsession` debe contener lo siguiente:

    <code>#!/bin/sh
    exec /usr/bin/startxfce4</code>

Ahora, cada vez que vaya a iniciar sesión, lo hará en modo gráfico de manera
automática, si no desea reiniciar para ver los resultados, utilice el comando
`startx`.

Espero en los siguientes días ir informando acerca del desempeño de Ubuntu con
el entorno de escritorio Xfce en una laptop de bajos recursos. Seguidamente
espero poder exponer una instalación bastante ligera pero utilizando el entorno
de escritorio [Enligtenment](http://www.enlightenment.org/).

### Referencias

[Debian Packages](http://os-works.com/view/debian/).
