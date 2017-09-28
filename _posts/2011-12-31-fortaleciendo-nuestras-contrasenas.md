---
redirect_from: "/archivos/2011/12/31/fortaleciendo-nuestras-contrasenas/"
author: milmazz
comments: true
date: 2011-12-31 02:53:55
layout: post
slug: fortaleciendo-nuestras-contrasenas
title: Fortaleciendo nuestras contraseñas
categories:
- debian
- Seguridad
- Ubuntu
tags:
- cracklib
- GNU/Linux
- pam
- security
- sysadmin
---

Si una de las promesas que tiene para este cierre de año es fortalecer las
contraseñas en sus equipos personales, cambiarlas mensualmente y no repetir la
misma contraseña en al menos doce cambios. En este artículo se le explicará como
hacerlo sin tener que invertir una uva en ello, todo esto gracias al paquete
`libpam-cracklib` en Debian, el procedimiento mostrado debe aplicarse a otras
distribuciones derivadas de Debian.

Pareciese lógico que algunas de las mejores prácticas para el fortalecimiento de
las contraseñas son las siguientes:

  * Cambiar las contraseñas periódicamente.
  * Establecer una longitud mínima en las contraseñas.
  * Establecer **buenas** reglas para las nuevas contraseñas, es decir, mezcla entre letras mayúsculas, minúsculas, dígitos y caracteres alfanuméricos.
  * Mantener un histórico de las contraseñas usadas previamente, de ese modo, alentamos a los usuarios establecer nuevas contraseñas.
  * Indicarle a los usuarios que es **inaudito** que se anoten las contraseñas en un _post-it_ y se dejen pegadas en los monitores o incluso en las gavetas de sus archivadores.

El primer paso es instalar el paquete `libpam-cracklib`
    
    # apt-get install libpam-cracklib

A partir de la versión 1.0.1-6 de PAM se recomienda manejar la configuración vía
`pam-auth-update`. Por lo tanto, por favor tome un momento y lea la sección 8
del manual del comando `pam-auth-update` para aclarar su uso y ventajas.

    $ man 8 pam-auth-update

Ahora establezca una configuración similar a la siguiente, vamos primero con la
exigencia en la fortaleza de las contraseñas, para ello edite o cree el fichero
`/usr/share/pam-configs/cracklib`.

    Name: Cracklib password strength checking
    Default: yes
    Priority: 1024
    Conflicts: unix-zany
    Password-Type: Primary
    Password:
    	requisite	pam_cracklib.so retry=3 minlen=8 difok=3
    Password-Initial:
    	requisite	pam_cracklib.so retry=3 minlen=8 difok=3

**NOTA:** Le recomiendo leer la sección 8 del manual de `pam_cracklib` para
encontrar un mayor numero de opciones de configuración. Esto es solo un ejemplo.

En versiones previas el modulo `pam_cracklib` hacia uso del fichero
`/etc/security/opasswd` para conocer si la propuesta de cambio de contraseña no
había sido utilizada previamente. Dicha funcionalidad ahora corresponde al nuevo
modulo `pam_pwhistory`

Definamos el funcionamiento de `pam_pwhistory` a través del fichero
`/usr/share/pam-configs/history`.

    Name: PAM module to remember last passwords
    Default: yes
    Priority: 1023
    Password-Type: Primary
    Password:
    	requisite	pam_pwhistory.so use_authtok enforce_for_root remember=12 retry=3
    Password-Initial:
    	requisite	pam_pwhistory.so use_authtok enforce_for_root remember=12 retry=3

**NOTA:** Para mayor detalle de las opciones puede revisar la sección 8 del
manual de `pam_pwhistory`

Seguidamente proceda a actualizar la configuración de PAM vía `pam-auth-update`.

![pam-auth-update][pam-auth-update]

Una vez cubierta la fortaleza de las contraseñas nuevas y de evitar la
reutilización de las ultimas 12, de acuerdo al ejemplo mostrado, resta cubrir la
definición de los periodos de cambio de las contraseñas.

Para futuros usuarios debemos ajustar ciertos valores en el fichero
`/etc/login.defs`

    #
    # Password aging controls:
    #
    #       PASS_MAX_DAYS   Maximum number of days a password may be used.
    #       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
    #       PASS_WARN_AGE   Number of days warning given before a password expires.
    #
    PASS_MAX_DAYS   30
    PASS_MIN_DAYS   0
    PASS_WARN_AGE   5

Las reglas previas no aplicaran para los usuarios existentes, pero para este
tipo de usuarios podremos hacer uso del comando `chage` de la siguiente manera:
    
    # chage -m 0 -M 30 -W 5 ${user}

Donde el valor de `${user}` debe ser reemplazo por el _username_.

[pam-auth-update]: /images/2011-12-31-fortaleciendo-nuestras-contrasenas/pantallazo-300x132.png "pam-auth-update"
