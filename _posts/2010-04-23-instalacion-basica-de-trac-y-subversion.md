---
redirect_from: "/archivos/2010/04/23/instalacion-basica-de-trac-y-subversion/"
author: milmazz
comments: true
date: 2010-04-23 02:14:51
layout: post
slug: instalacion-basica-de-trac-y-subversion
title: Instalación básica de Trac y Subversion
categories:
- debian
- GNU/Linux
- Software
- subversion
tags:
- apache
- debian
- GNU/Linux
- postgresql
- scm
- Software
- subversion
- trac
---

En este artículo se pretenderá mostrarle el proceso de instalación de un ambiente de desarrollo que le permitirá hacerle seguimiento a su _proyecto personal_, de igual manera se le indicará el modo en el cual puede comenzar a utilizar un _sistema de control de versiones_. Todas las indicaciones mostradas en este documento han sido probadas en la distribución Debian GNU/Linux 5.0 (nombre código _Lenny_).

La herramienta de seguimiento o manejo del proyecto que se procederá a instalar es [Trac](http://trac.edgewall.com), el sistema de control de versiones que se presentará será [Subversion](http://subversion.tigris.org). Todo lo anterior se presentará vía _Web_ haciendo uso del servidor [Apache](http://httpd.apache.org), de manera adicional se utilizará el servidor de bases de datos [PostgreSQL](http://www.postgresql.org) como _backend_ de Trac.

## Dependencias

En primer lugar proceda a instalar las siguientes dependencias.
    
    # aptitude install apache2 \
    libapache2-mod-python \
    postgresql \
    subversion \
    python-psycopg2 \
    libapache2-svn \
    python-subversion \
    trac

La versión de Trac que se encuentra en los archivos de Debian _Lenny_ es estable (**0.11.1**). Sin embargo, si usted compara esta versión con lo publicado en el sitio oficial de Trac, podrá encontrar que existen nuevas versiones estables de mantenimiento que contienen correcciones a errores de programación y algunas nuevas funcionalidades de bajo impacto, para el momento de la redacción de este artículo se encuentra la versión [0.11.7](http://ftp.edgewall.com/pub/trac/Trac-0.11.7.tar.gz). Es recomendable que utilice el paquete `trac` desde el archivo [backports](http://backports.org) de Debian.

    # aptitude -t lenny-backports install trac

Si desea usar el paquete proveniente del archivo [backports](http://backports.org) le recomiendo leer las [instrucciones](http://backports.org/dokuwiki/doku.php?id=instructions) de uso de este repositorio de paquetes.

Con el fin de mantener este artículo lo más sencillo y conciso posible se describirá la versión que viene por defecto con la distribución utilizada en este caso.

### Versión de desarrollo de Trac

Si desea conocer algunas características interesantes que se han agregado a Trac en las nuevas versiones, o si su interés particular es
examinar las bondades que le ofrece Trac en su versión de desarrollo puede hacer uso del comando `pip`, si no tiene instalado [pip](http://pip.openplans.org/) proceda como sigue:

    # aptitude install python-setuptools
    # easy_install pip

Proceda a instalar la versión de desarrollo de Trac.

    # pip install https://svn.edgewall.org/repos/trac/trunk

## Creación de usuario y base de datos

Antes de proceder con la instalación de Trac se debe establecer el usuario y la base de datos que se utilizará para trabajar.

En el siguiente ejemplo el usuario de la base de datos PostgreSQL que utilizará Trac será `trac_user`, su contraseña será `trac_passwd`. El uso de la contraseña lo del usuario `trac_user` lo veremos más tarde al proceder a configurar el ambiente de Trac.

    # su postgres
    $ createuser \
    --no-superuser \
    --no-createdb \
    --no-createrole \
    --pwprompt \
    --encrypted trac_user
    Enter password for new role:
    Enter it again:
    CREATE ROLE

Nótese que se **debe** utilizar el usuario `postgres` (u otro usuario con los privilegios necesarios) del sistema para proceder a crear los usuarios.

Ahora procedemos a crear la base de datos `trac_dev`, cuyo dueño será el usuario `trac_user`.

    $ createdb --encoding UTF8 --owner trac_user trac_dev

## Ambiente de Trac

Creamos el directorio `/srv/www` que será utilizado para prestar servicios _Web_, para mayor referencia acerca de esta elección se le recomienda leer el documento [Filesystem Hierarchy Standard](http://www.pathname.com/fhs/pub/fhs-2.3.html) en su versión 2.3.

    $ sudo mkdir -p /srv/www
    $ cd /srv/www

Generamos el ambiente de Trac. Básicamente se deben contestar ciertas preguntas como:

  * Nombre del proyecto.
  * Enlace con la base de datos que se utilizará.
  * Sistema de control de versiones a utilizar (por defecto será SVN).
  * Ubicación absoluta del repositorio a utilizar.
  * Ubicación de las plantillas de Trac.

En el siguiente ejemplo se omitirán los comentarios brindados por el comando `trac-admin`.

    # trac-admin project initenv
    Creating a new Trac environment at /srv/www/project
    
    ...
    
    Project Name [My Project]> Demo
    
    ...
    
    Database connection string [sqlite:db/trac.db]> 
    postgres://trac_user:trac_passwd@localhost:5432/trac_dev
    
    ...
    
    Repository type [svn]>
    
    ...
    
    Path to repository [/path/to/repos]> /srv/svn/project
    
    ...
    
    Templates directory [/usr/share/trac/templates]>
    
    Creating and Initializing Project
     Installing default wiki pages
    
    ...
    
    ---------------------------------------------------------
    Project environment for 'Demo' created.
    
    You may now configure the environment by editing the file:
    
      /srv/www/project/conf/trac.ini
    
    ...
    
    Congratulations!

Se ha culminado la instalación del ambiente de Trac, ahora procederemos a crear el ambiente de subversion.

## Subversion: Sistema Control de Versiones

La puesta a punto del sistema de control de versiones es bastante sencilla, se resume en los siguientes pasos.

Creación del espacio para prestar el servicio SVN, de igual manera se seguirá los lineamientos planteados en el FHS v2.3 mencionado previamente.

    # mkdir -p /srv/svn

Seguidamente crearemos el proyecto _subversion_ `project` y haremos un importe inicial con la estructura básica de un proyecto de desarrollo de software.

    $ cd /srv/svn
    # svnadmin create project
    $ mkdir -p /tmp/project/{trunk,tags,branches}
    $ tree /tmp/project/
    /tmp/project/
    ├── branches
    ├── tags
    └── trunk
    # svn import /tmp/project/ \
    file:///srv/svn/project/ \
    -m 'Importe inicial'
    
    Adding         /tmp/project/trunk
    Adding         /tmp/project/branches
    Adding         /tmp/project/tags
    
    Committed revision 1.
    
## Apache: Servidor Web

Es hora de configurar los _sitios virtuales_ que utilizaremos tanto para Trac como para subversion.

A continuación se presenta la configuración _básica_ de Trac.

    # cat > /etc/apache2/sites-available/trac.example.com <<EOF
    > <VirtualHost *>
    > ServerAdmin webmaster@localhost
    > ServerName trac.example.com
    > CustomLog /var/log/apache2/trac.example.com/access.log combined
    > ServerSignature Off
    > ErrorLog /var/log/apache2/trac.example.com/error.log
    > LogLevel warn
    > DocumentRoot /srv/www/project
    > <Location />
    > SetHandler mod_python
    > PythonInterpreter main_interpreter
    > PythonHandler trac.web.modpython_frontend
    > PythonOption TracEnv /srv/www/project
    > PythonOption TracUriRoot /
    > </Location>
    > </VirtualHost>
    > EOF

Ahora veamos la configuración _básica_ de nuestro proyecto subversion.

    # cat > /etc/apache2/sites-available/svn.example.com <<EOF
    > <VirtualHost *>
    > ServerAdmin webmaster@localhost
    > ServerName svn.example.com
    > CustomLog /var/log/apache2/svn.example.com/access.log combined
    > ServerSignature Off
    > ErrorLog /var/log/apache2/svn.example.com/error.log
    > LogLevel warn
    > DocumentRoot /srv/svn/project
    > <Location />
    > DAV svn
    > SVNPath /srv/svn/project
    > </Location>
    > </VirtualHost>
    > EOF

De acuerdo a la configuración mostrada es necesario crear los directorios `/var/log/apache2/svn.example.com` y `/var/log/apache2/trac.example.com` para mantener por separado el registro de accesos y errores de los _sitios virtuales_ recién creados. De
lo contrario no podremos iniciar el servidor _Web_ Apache.

    # mkdir /var/log/apache2/{svn,trac}.example.com

Activamos el módulo `DAV` necesario para cumplir con la configuración mostrada para el _sitio virtual_ `svn.example.com`.

    # a2enmod dav

Activamos los sitios virtuales recién creados.

    # a2ensite trac.example.com
    # a2ensite svn.example.com

Como la puesta en marcha y configuración de un servidor DNS escapa a los fines de este artículo, simplemente definiremos los _hosts_ en la tabla estática que se encuentra definida en el archivo `/etc/hosts` de la siguiente manera.

    # cat >> /etc/hosts <<EOF
    > 127.0.0.1 trac.example.com trac
    > 127.0.0.1 svn.example.com svn
    > EOF

Finalmente debemos forzar la recarga de la configuración del servidor _Web_ Apache, esto con el fin de cargar el módulo `DAV` y los sitios virtuales definidos, es decir, `trac.example.com` y `svn.example.com`.

    # /etc/init.d/apache2 force-reload

**¡Felicitaciones!**, ya puede ir a su navegador favorito y colocar cualquiera de los sitios virtuales que acaba de definir.

## Recomendaciones

Una vez que haya llegado a este sección deberá comenzar a modificar adecuadamente los permisos para el usuario y grupo `www-data` (Servidor Web Apache) para que escriba en las ubicaciones que sea necesario tanto en Trac como en subversion.

La configuración de Trac podrá encontrarla en `/srv/www/project/conf/trac.ini`, para mayor detalle acerca de la configuración de este fichero se le recomienda leer el documento [TracIni](http://trac.edgewall.org/wiki/TracIni).

Si en su proyecto prevé que participarán otras personas, es recomendable establecer notificaciones de _tickets_ y cambios en el
repositorio _subversion_, para esto último deberá revisar el _hook_ `post-commit` y la documentación del paquete [libsvn-notify-perl]({% post_url 2010-04-24-subversion-notificaciones-via-correo-electronico %}) que le ofrece una extraordinaria cantidad de opciones.

Conozca el entorno de Trac y Subversion. Si desea agregar nuevas funciones a su instalación le recomiendo visitar el sitio [Trac Hacks](http://trac-hacks.org/).

Espero poder mostrarles en próximos artículos algunas configuraciones más avanzadas en Trac, incluyendo la instalación, configuración y uso de _plugins_.
