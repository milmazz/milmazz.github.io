---
title: 'Debian: Bienvenido al Sistema Operativo Universal (Parte II)'
author: milmazz
date: 2006-05-02 12:12:24
categories:
  - debian
  - GNU/Linux
tags:
  - debian
  - GNU/Linux
slug: debian-bienvenido-al-sistema-operativo-universal-parte-ii
redirect_from: /archivos/2006/05/02/debian-bienvenido-al-sistema-operativo-universal-parte-ii/
---

Esta serie de anotaciones comenzo con la entrada [Debian: Bienvenido al Sistema Operativo Universal (Parte I)](/article/2006/04/25/debian-bienvenido-al-sistema-operativo-universal-parte-i/).

Después de escribir en la tabla de particiones el esquema de particionamiento descrito en la [parte anterior](/article/2006/04/25/debian-bienvenido-al-sistema-operativo-universal-parte-i/), el sistema base Debian comenzo a instalarse. Posterior a la Bienvenida al nuevo sistema Debian, reinicie y comence a configurar el _sistema base debian_, en la sección de selección de programas Debian escogí la última opción que nos brinda el asistente, selección manual de paquetes, luego configure las fuentes de aptitude y enseguida inicie la instalación de paquetes puntuales, los cuales describiré a continuación, de manera breve.

Como lo que tenía a mano era el CD Debian GNU/Linux testing _Sarge_- Official Snapshot i386 Binary-1, lo primero que hice fue actualizar a Sarge, seguidamente cambie las fuentes del fichero `/etc/apt/sources.list` a Etch, actualice la lista de paquetes disponibles e inmediatamente hice un `aptitude dist-upgrade`, el cambio de una rama a otra fué de lo más normal, no genero problema alguno.

**Nota:** No he descrito el proceso de instalación del sistema base de manera detallada ya que existe **suficiente** información en el sitio oficial de [Debian](http://debian.org/), si lo desea, puede ver este [video tutorial de instalación de Debian Sarge](http://agora.pucp.edu.pe/ftp/pub/linux/videos/tutor_install/debian/debian.html) (aprox. 54MB.), en este video se explica como instalar el Entorno de Escritorio predeterminado que ofrece el asistente, no es el caso que explico en esta entrada, puesto que vamos a generar un Entorno de Escritorio de acuerdo a nuestras necesidades particulares.

Si tiene alguna duda acerca de la funcionalidad de un paquete en particular, puede consultar la descripción del mismo al hacer uso del comando `aptitude show _package-name_`, en donde _package-name_ es el nombre del paquete en cuestión.

En los siguientes pasos haré uso intensivo de `aptitude`, anteriormente ya he explicado las ventajas que presenta `aptitude` sobre los comandos `apt-get` y sobre la interfaz gráfica Synaptic, puede encontrar mayor información en los artículos:

  * [aptitude, ¿aún no lo usas?](/article/2005/07/28/aptitude-%C2%BFaun-no-lo-usas/)
  * [Registro de la segunda charla en el canal #ubuntu-es](/article/2005/09/17/registro-de-la-segunda-charla-en-el-canal-ubuntu-es/)

## Sistema X Window

Instalando los componentes **esenciales** para el Sistema X Window.

    # aptitude install x-window-system-core

## GNOME

Instalando los componentes **esenciales** para el entorno de escritorio GNOME.

    # aptitude install gnome-core

## GNOME Display Manager

Si usted hubiese instalado el paquete `x-window-system`, metapaquete que incluye todos los componentes para el Sistema X Window, se instalaría por defecto XDM (_X Display Manager_), normalmente debería recurrir a la línea de comandos para resolver los problemas de configuración de este manejador, mientras que con GDM (_GNOME Display Manager_) no debe preocuparse por ello, puede personalizarlo o solucionar los problemas sin recurrir a la línea de comandos. De manera adicional, puede mejorar su presentación con la instalación de temas.

    # aptitude install gdm gdm-themes

## Mensajería Instantánea, IRC, Jabber

### GAIM

Todo lo anterior se puede encontrar al instalar [GAIM](http://gaim.sourceforge.net/).

    # aptitude install gaim gaim-themes

### irssi

Si le agrada utilizar IRC modo texto, puede instalar [irssi](http://www.irssi.org/).

    # aptitude install irssi

### Centericq

[Centericq](http://konst.org.ua/centericq/) es un cliente de mensajería instantánea multiprotocolo, soporta ICQ2000, Yahoo!, AIM, IRC, MSN, Gadu-Gadu y Jabber.

    # aptitude install centericq

## Programas para la manipulación de imágenes

[GIMP](http://gimp.org) por defecto no puede abrir ficheros SVG, si desea manipularlos desde GIMP y no desde [Inkscape](http://www.inkscape.org/) puede hacer uso del paquete `gimp-svg`.

    # aptitude install gimp gimp-python gimp-svg inkscape

## Navegador Web

Definitivamente [Firefox](http://www.mozilla.com/firefox).

    # aptitude install firefox firefox-locale-es-es

## Creación de CDs y DVDs

    # aptitude install k3b cdrdao

Para quienes quieran su version de [K3b](http://www.k3b.org/) en español pueden instalar el paquete k3b-i18n, yo no lo considere necesario puesto que aporta **11,5MB**, inútiles desde mi punto de vista.

## Cliente Bittorrent

No le recomiendo instalar el cliente bittorrent Azureus, consume demasiados recursos, acerca de ello explico brevemente en el artículo [Clientes Bittorrent](/article/2005/12/06/clientes-bittorrent/).

    # aptitude install freeloader

## Lector de feeds

Normalmente utilizo [Liferea](http://liferea.sourceforge.net/). También cabe la posibilidad de utilizar el servicio que presta [Bloglines](http://bloglines.com/).

    # aptitude install liferea

## Editor

    # aptitude install vim-full

## Cliente de correo electrónico

Para la fecha, a la rama _testing_ de Debian **no** ingresa la versión 1.5 del cliente de correo Thunderbird (mi favorito), así que vamos a instalarlo manualmente.

En primer lugar deberá descargar (se asume que la descarga se realizará al escritorio) la última versión del cliente de correo, el paquete empaquetado y comprimido lo encontrará en el [sitio oficial de Thunderbird](http://www.mozilla.com/thunderbird/).

Seguidamente, proceda con los siguientes comandos.

    # tar -C /opt/ -x -v -z -f ~/Desktop/thunderbird*.tar.gz
    # ln -s /opt/thunderbird/thunderbird /usr/bin/thunderbird

### Instalar Diccionario en español

    wget -c http://downloads.mozdev.org/dictionaries/spell-es-ES.xpi

Aplicaciones -> Herramientas del Sistema -> Root Terminal. Desde allí procedará a ejecutar el comando `thunderbird`.

Desde Thunderbird, seleccione la opción **Extensiones** del menú **Herramientas**. Seguidamente proceda a dar click en el botón **Instalar** y posteriormente busque la ruta del paquete que contiene el diccionario.

### Thunderbird + GPG = Enigmail

Esta excelente extensión le permitira cifrar y descifrar correos electrónicos, a su vez, le permitirá autenticar usuarios usando OpenPGP.

    wget -c http://releases.mozilla.org/pub/mozilla.org/extensions/enigmail/enigmail-0.94.0-mz+tb-linux.xpi

Como usuario normal, proceda a invocar el cliente de correo electrónico Thunderbird, seleccione la opción **Extensiones** del menú **Herramientas** y proceda a instalar la extensión en cuestión, similar al proceso seguido para lograr instalar el diccionario.

## Reproductor de videos

Aunque existen reproductores muy buenos como el [VLC](http://www.videolan.org/vlc/) y [XINE](http://xinehq.de/), mi reproductor favorito es, sin lugar a dudas, [MPlayer](http://www.mplayerhq.hu/). La mejor opción es compilarlo, aún en la misma página oficial del proyecto MPlayer lo recomiendan, existe mucha documentación al respecto. Sin embargo, si no tiene tiempo para documentarse puede seguir los siguientes pasos, le advierto que el rendimiento quizá no sea el mismo que al compilar el programa.

    # echo "deb ftp://ftp.nerim.net/debian-marillat/ etch main" >> /etc/apt/sources.list
    # aptitude update
    # aptitude install mplayer-386 w32codecs

## Extensiones para Mozilla Firefox

Estas son las que he instalado hasta ahora, siempre uso un poco más, puede encontrarlas en la sección [Firefox Add-ons](https://addons.mozilla.org/?application=firefox) del sitio oficial.

  * Answers
  * del.icio.us
  * FireFoxMenuButtons
  * Colorful Tabs
  * Tab Mix Plus

## Gestor de Arranque

Vamos a personalizar un poco el gestor de arranque GRUB.

    # aptitude install grub-splashimages
    $ cd /boot/grub/splashimages

En este instante le recomiendo escoger alguno de los motivos que incluye el paquete grub-splashimages, una vez hecho esto, proceda a realizar lo siguiente.

    $ cd /boot/grub/
    # ln -s splashimages/image.xpm.gz  splash.xpm.gz
    # update-grub

En donde, evidentemente, debe cambiar el nombre del fichero _image.xpm.gz_ por el nombre de fichero de la imagen que le haya gustado en /boot/grub/splashimages.
