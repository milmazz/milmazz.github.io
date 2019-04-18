---
title: Cómo cambiar entre versiones de JAVA bajo Breezy
author: milmazz
date: 2005-11-28 12:16:40
categories:
  - Ubuntu
tags:
  - java
  - Ubuntu
slug: como-cambiar-entre-versiones-de-java-bajo-breezy
redirect_from: /archivos/2005/11/28/como-cambiar-entre-versiones-de-java-bajo-breezy/
---

Si usted tiene instaladas múltiples versiones de JAVA bajo Breezy, puede cambiar fácilmente entre dichas versiones cuando usted lo desee.

Simplemente debe hacer uso del comando `update-alternatives --config java` y luego podrá escoger su versión de java preferida.

    $ sudo update-alternatives --config java

    There are 3 alternatives which provide `java'.

      Selection    Alternative
    -----------------------------------------------
          1        /usr/bin/gij-wrapper-4.0
     +    2        /usr/lib/jvm/java-gcj/bin/java
    *     3        /usr/lib/j2re1.5-sun/bin/java

    Press enter to keep the default[*], or type selection number:

Si usted es amante de las soluciones gráficas, no se preocupe, existe una alternativa, con `galternatives` podrá elegir qué programa le prestará un servicio en particular, para instalar `galternatives` debe teclear el siguiente comando:

    $ sudo aptitude install galternatives

Para ejecutarlo debe hacer lo siguiente:

    $ sudo galternatives

Su interfaz de uso es muy sencilla, si desea cambiar la versión de JAVA que desea utilizar simplemente escoga **java** en la sección de **alternativas**, seguidamente seleccione la versión que más crea conveniente en la sección de **opciones**
