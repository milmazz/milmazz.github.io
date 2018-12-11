---
redirect_from: "/archivos/2010/11/15/instalando-dependencias-no-libres-de-java-en-ambientes-pbuilder/"
author: milmazz
comments: true
date: 2010-11-15 17:14:37
layout: post
slug: instalando-dependencias-no-libres-de-java-en-ambientes-pbuilder
title: Instalando dependencias no-libres de JAVA en ambientes pbuilder
categories:
- debian
- Ubuntu
tags:
- debconf
- debian
- java
- pbuilder
---

El día de hoy asumí la construcción de unos paquetes internos compatibles con
[Debian][] 5.0 (a.k.a. Lenny) que anteriormente eran responsabilidad de
ex-compañeros de labores. El paquete en cuestión posee una dependencia
**no-libre**, `sun-java6-jre`. En este artículo se describirá como lograr
adecuar su configuración de `pbuilder` para la correcta construcción del
paquete.

Asumiendo que tiene un configuración similar a la siguiente:

	$ cat /etc/pbuilderrc
	MIRRORSITE=http://example.com/debian
	DEBEMAIL="Maintainer Name <mail@example.com>"
	DISTRIBUTION=lenny
	DEBOOTSTRAP="cdebootstrap"
	COMPONENTS="main contrib non-free"

Para mayor información sobre estas opciones sírvase leer:

	$ man 5 pbuilderrc

Mientras intenta compilar su paquete en el ambiente proporcionado por `pbuilder`
el proceso fallará ya que no se mostró la ventana para aceptar la licencia de
JAVA. Podrá observar en el registro de la construcción del build un mensaje
similar al siguiente:

	Unpacking sun-java6-jre (from .../sun-java6-jre_6-20-0lenny1_all.deb) ...

	sun-dlj-v1-1 license could not be presented
	try 'dpkg-reconfigure debconf' to select a frontend other than noninteractive

	dpkg: error processing /var/cache/apt/archives/sun-java6-jre_6-20-0lenny1_all.deb (--unpack):
	subprocess pre-installation script returned error exit status 2

Para evitar esto altere la configuración del fichero `pbuilderrc` de la
siguiente manera:

	$ cat /etc/pbuilderrc
	MIRRORSITE=http://example.com/debian
	DEBEMAIL="Maintainer Name <mail@example.com>"
	DISTRIBUTION=lenny
	DEBOOTSTRAP="cdebootstrap"
	COMPONENTS="main contrib non-free"
	export DEBIAN_FRONTEND="readline"

Una vez alterada la configuración podrá interactuar con las opciones que le
ofrece `debconf`.

Ahora bien, si usted constantemente tiene que construir paquetes con
dependencias **no-libres** como las de JAVA, es probable que le interese lo que
se menciona a continuación.

Si lee detenidamente la página del manual de `pbuilder` en su sección 8 podrá
encontrar lo siguiente:

	$ man 8 pbuilder
	...
	--save-after-login
	--save-after-exec
	Save the chroot image after exiting from the chroot instead of deleting changes.  Effective for login and execute session.
	...

Por lo tanto, usaremos esta funcionalidad que ofrece `pbuilder` para insertar
valores por omisión en la base de datos de `debconf` para que no se nos pregunte
si deseamos aceptar la licencia de JAVA:

	# pbuilder login --save-after-login
	I: Building the build Environment
	I: extracting base tarball [/var/cache/pbuilder/base.tgz]
	I: creating local configuration
	I: copying local configuration
	I: mounting /proc filesystem
	I: mounting /dev/pts filesystem
	I: Mounting /var/cache/pbuilder/ccache
	I: policy-rc.d already exists
	I: Obtaining the cached apt archive contents
	I: entering the shell
	File extracted to: /var/cache/pbuilder/build//27657

	pbuilder:/# cat > java-license << EOF
	> sun-java6-bin shared/accepted-sun-dlj-v1-1 boolean true
	> sun-java6-jdk shared/accepted-sun-dlj-v1-1 boolean true
	> sun-java6-jre shared/accepted-sun-dlj-v1-1 boolean true
	> EOF
	pbuilder:/# debconf-set-selections < java-license
	pbuilder:/# exit
	logout
	I: Copying back the cached apt archive contents
	I: Saving the results, modifications to this session will persist
	I: unmounting /var/cache/pbuilder/ccache filesystem
	I: unmounting dev/pts filesystem
	I: unmounting proc filesystem
	I: creating base tarball [/var/cache/pbuilder/base.tgz]
	I: cleaning the build env
	I: removing directory /var/cache/pbuilder/build//27657 and its subdirectories

[Debian]: http://www.debian.org
