---
title: COMO instalar e17 desde los repositorios de shadoi en Breezy
author: milmazz
date: 2005-10-22 13:55:41
categories:
  - Ubuntu
tags:
  - Enlightenment
  - Ubuntu
slug: como-instalar-e17-desde-los-repositorios-de-shadoi-en-breezy
redirect_from: /archivos/2005/10/22/como-instalar-e17-desde-los-repositorios-de-shadoi-en-breezy/
---

**Nota:** Antes que nada es importante resaltar que E17 está todavía en
desarrollo. Si desea intentar utilizar E17, sepa que este software es todavía un
versión pre-alfa, que aún esta en constante desarrollo y por tanto su uso puede
implicar algunos riesgos.

[Enlightenment](http://www.enlightenment.org/) DR17 combina características
presentes en los manejadores de ventanas como en los manejadores de ficheros.
Provee una interfaz gráfica de usuario que le permitirá manejar los elementos
del escritorio, al igual que ficheros y ventanas. El entorno gráfico de
Enlightenment es realmente impresionante, es muy agradable para el usuario,
también es muy configurable.

Después de una breve introducción, vamos a entrar en acción.

Hace ya algunos días atrás [shadoi](http://shadoi.soulmachine.net/) anunciaba lo
siguiente:

> Once that new server is in place and all the sites have been moved over, I'll
> be quickly adding support for more Debian architectures, distributions and
> derivatives like **Ubuntu**.

Posteriormente shadoi (aka Blake Barnett) confirma la noticia.

Ahora para instalar E17 solo necesitará de **4** pasos. La información que será
mostrada a continuación es tomada del [wiki](http://www.soulmachine.net/wiki/)
de [shadoi](http://shadoi.soulmachine.net/).

## Paso #1

Agregar la siguiente línea al fichero `/etc/apt/sources.list`:

    deb http://soulmachine.net/breezy/ unstable/

También debe agregar lo siguiente al fichero `/etc/apt/preferences` (si no
existe dicho fichero proceda a crearlo).

    Package: enlightenment
    Pin: version 0.16.999*
    Pin-Priority: 999

    Package: enlightenment-data
    Pin: version 0.16.999*
    Pin-Priority: 999

## Paso #2

Instalar la clave pública del repositorio, para ello desde la consola escriba lo
siguiente:

    $ wget soulmachine.net/public.key
    $ sudo apt-key add public.key

## Paso #3

Actualice la lista de paquetes disponibles, para ello desde la consola escriba
lo siguiente:

    $ sudo aptitude update

## Paso #4

Instale enligtenment, para ello desde la consola escriba lo siguiente:

    $ sudo aptitude install <ins datetime="2005-12-10T05:54:58+00:00">enlightenment=0.16.999.018-1 enlightenment-data</ins>

### Notas

Aunque [shadoi](http://shadoi.soulmachine.net/) ha anunciado que el paquete de
_evidence_ (gestor de ficheros especialmente desarrollado para ser usado con
_enlightenment_) pronto lo tendrá listo, por ahora puede hacer uso de [esta
versión](http://surfnet.dl.sourceforge.net/sourceforge/evidence/evidence_0.9.8-20050305-hoaryGMW_i386.deb),
después de finalizada la descarga debe escribir en consola lo siguiente:

    $ sudo dpkg -i evidence_0.9.8-20050305-hoaryGMW_i386.deb

El paquete que se ha instalado previamente aunque fue construido en principio
para la versión de Ubuntu Hoary, funciona en Breezy. Para la versión reciente de
Ubuntu aún no está construido este paquete en particular.

También es importante resaltar que E17 brinda soporte al _castellano_.
