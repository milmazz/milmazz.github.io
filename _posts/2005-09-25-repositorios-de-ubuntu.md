---
title: Repositorios de Ubuntu
author: milmazz
date: 2005-09-25 13:52:57
categories:
  - GNU/Linux
  - Ubuntu
tags:
  - GNU/Linux
  - Ubuntu
slug: repositorios-de-ubuntu
---

Desde hace unas horas hasta hace poco el servidor principal que mantiene los
archivos de los paquetes binarios y fuentes estaba caído.

Obteniendo respuesta desde
[http://archive.ubuntu.com/ubuntu](http://archive.ubuntu.com/ubuntu).

    <code>$ wget http://archive.ubuntu.com
    --09:56:27--  http://archive.ubuntu.com/
               => `index.html'
    Resolving archive.ubuntu.com... 82.211.81.151, 82.211.81.182
    Connecting to archive.ubuntu.com[82.211.81.151]:80... failed: Connection refused.
    Connecting to archive.ubuntu.com[82.211.81.182]:80... failed: Connection refused.</code>

En cambio, el servicio por `ftp` **si** estaba habilitado. Obteniendo respuesta
desde [ftp://archive.ubuntu.com/ubuntu](ftp://archive.ubuntu.com/ubuntu)

    <code>$ wget ftp://archive.ubuntu.com
    --09:58:24--  ftp://archive.ubuntu.com/
               => `.listing'
    Resolving archive.ubuntu.com... 82.211.81.182, 82.211.81.151
    Connecting to archive.ubuntu.com[82.211.81.182]:21... connected.
    Logging in as anonymous ... Logged in!
    ==> SYST ... done.    ==> PWD ... done.
    ==> TYPE I ... done.  ==> CWD not needed.
    ==> PASV ... done.    ==> LIST ... done.

        [ < =>                                 ] 64            --.--K/s

    09:58:24 (48.75 KB/s) - `.listing' saved [64]

    Removed `.listing'.
    Wrote HTML-ized index to `index.html' [302].</code>

Ahora bien, en este caso simplemente bastaba con cambiar la entrada `http` por
`ftp` en el fichero /etc/apt/sources. Para evitar cualquier tipo de
inconvenientes en el futuro, es recomendable hacer uso de _sitios espejo_ o
_mirrors_.

En [https://wiki.ubuntu.com/Archive](https://wiki.ubuntu.com/Archive) encontrará
toda la información necesaria, si utiliza los repositorios del proyecto [Ubuntu
Backport](http://backports.ubuntuforums.org/) es recomendable que vea su
[sección de las URL de los
Repositorios](http://backports.ubuntuforums.org/url.php).

**NOTA:** Siempre es recomendable hacer uso de _sitios espejos_ puesto que estos
presentan menos demanda que los sitios oficiales de los proyectos.
