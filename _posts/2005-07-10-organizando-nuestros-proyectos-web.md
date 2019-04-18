---
title: Organizando nuestros proyectos web
author: milmazz
date: 2005-07-10 23:53:01
categories:
  - GNU/Linux
  - Software
  - Ubuntu
tags:
  - GNU/Linux
  - Software
  - Ubuntu
slug: organizando-nuestros-proyectos-web
redirect_from: /archivos/2005/07/10/organizando-nuestros-proyectos-web/
---

Antes de plantearme el rediseño de este sitio, pensé en organizar ciertas cosas para no complicarme la vida. En primera instancia procedi a instalar el servidor [Apache](http://apache.org/), el soporte del lenguaje [PHP](http://www.php.net/), el servidor de bases de datos [MySQL](http://www.mysql.com/) y finalmente el [phpMyAdmin](http://www.phpmyadmin.net/).

Lo anterior se resume de la siguiente manera en mi querido [Ubuntu Linux](http://ubuntulinux.org/).

    $ sudo aptitude install apache2
    $ sudo aptitude install mysql-server
    $ sudo aptitude install php4
    $ sudo aptitude install libapache2-mod-auth-mysql
    $ sudo aptitude install php4-mysql
    $ sudo /etc/init.d/apache2 restart
    $ sudo aptitude install phpmyadmin

Recuerde después de instalar el servidor de bases de datos MySQL establecer la contraseña del usuario root de dicho servidor por seguridad. Si no sabe como hacerlo a través de la línea de comandos existe la alternativa de establecer la contraseña utilizando phpMyAdmin.

Por defecto el servidor Apache habilita el directorio /var/www/ como directorio principal, el problema que se plantearía el trabajar en dicho directorio es que cada vez que tuviese que realizar cambios a los ficheros tendría que hacer uso de la cuenta de superusuario o root, lo cual no me agrada mucho, así que para evitar lo mencionado previamente tenía dos opciones.

La primera de ellas era modificar el fichero /etc/apache2/apache.conf y habilitar que todos los usuarios tuviesen la posibilidad de publicar sus documentos dentro del directorio /home/*/public_html, esta opción no me llamaba mucho la atención, adicionalmente, la dirección queda de cierta manera algo extensa (p.ej. http://localhost/~usuario/proyectoweb), así que opte por recurrir a una segunda opción, la cual describiré detalladamente a continuación.

En primera instancia creé dos directorios, `public_html` como subdirectorio de $HOME, el segundo, `milmazz` como subdirectorio de `public_html`.

    $ mkdir $HOME/public_html
    $ mkdir $HOME/public_html/milmazz

Seguidamente procedi a crear el fichero `/etc/apache2/sites-enabled/001-milmazz`, este fichero contendrá las directivas necesarias para nuestro proyecto desarrollado en un ambiente local.

    <VirtualHost *>
      DocumentRoot "/home/milmazz/public_html/milmazz/"
      ServerName milmazz.desktop

      <Directory "/home/milmazz/public_html/milmazz/">
        Options Indexes FollowSymLinks MultiViews
        AllowOverride None
        Order deny,allow
        Deny from all
        Allow from 127.0.0.0/255.0.0.0 ::1/128
      </Directory>
    </VirtualHost>

Ahora bien, necesitamos crear el _host_ para que la directiva realmente redireccione al directorio /home/milmazz/public_html/milmazz/ simplemente tecleando milmazz.desktop en la barra de direcciones de nuestro navegador preferido. Por lo tanto, el siguiente paso es editar el fichero /etc/hosts y agregar una línea similar a `127.0.0.1 milmazz.desktop`, note que el segundo parámetro especificado es exactamente igual al especificado en la directiva `ServerName`.

Seguidamente procedi a instalar [WordPress](http://wordpress.org/) (Sistema Manejador de Contenidos) en el directorio $HOME/public_html/milmazz y todo funciona de maravilla.

## Evitando el acceso de usuarios no permitidos

Si queremos restringir aún más el acceso al directorio que contendrá los ficheros de nuestro proyecto web, en este ejemplo, el directorio es $HOME/public_html/milmazz/, realizaremos lo siguiente:

    $ touch $HOME/public_html/milmazz/.htaccess
    $ nano $HOME/public_html/milmazz/.htaccess

Dentro del fichero .htaccess insertamos las siguientes lineas.

    AuthUserFile /var/www/.htpasswd
    AuthGroupFile /dev/null
    AuthName "Acceso Restringido"
    AuthType Basic

    require valid-user

Guardamos los cambios realizados y seguidamente procederemos a crear el fichero que contendrá la información acerca de los usuarios que tendrán acceso al directorio protegido, también se especificaran las contraseñas para validar la entrada. El fichero a editar vendra dado por la directiva `AuthUserFile`, por lo tanto, en nuestro caso el fichero a crear será /var/www/.htpasswd.

    htpasswd -c /var/www/.htpasswd milmazz

Seguidamente introducimos la contraseña que se le asignará al usuario milmazz, la opción `-c` mostrada en el código anterior simplemente se utiliza para crear el fichero /var/www/.htpasswd, por lo tanto, cuando se vaya a autorizar a otro usuario cualquiera la opción `-c` debe ser ignorada.

## Estableciendo la contraseña del usuario root del servidor de bases de datos a través de phpMyAdmin.

Si usted no sabe como establecer en línea de comandos la contraseña del usuario root en el servidor de bases de datos MySQL es posible establecerla en modo gráfico haciendo uso de phpMyAdmin.

A continuacion se describen los pasos que deberá seguir:

  1. Acceder a la interfaz del phpMyAdmin, para lograrlo escribe en la barra de direcciones de tu navegador http://localhost/phpmyadmin.
  2. Selecciona la opción de **Privilegios**, ésta se ubica en el menú principal.
  3. Debemos editar los dos últimos registros, uno representa el superusuario cuyo valor en el campo _servidor_ será equivalente al nombre de tu máquina, el otro registro a editar es aquel superusuario cuyo valor en el campo _servidor_ es igual a _localhost_. Para editar los registros mencionados anteriormente simplemente deberá presionar sobre la imagen que representa un **lapíz**.
  4. Los únicos campos que modificaremos de los registros mencionados previamente son aquellos que tienen relación con la contraseña. Recuerde que para guardar los cambios realizados al final debe presionar el botón **Continúe**. Si deseas evitar cualquier confusión en cuanto al tema de las contraseñas es recomendable que establezcas la misma en ambos registros.

## Agradecimientos

A [José Parella](http://www.qtpd.com/jose/) por sus valiosas indicaciones.
