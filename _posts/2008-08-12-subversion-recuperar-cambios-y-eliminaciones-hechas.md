---
redirect_from: "/archivos/2008/08/12/subversion-recuperar-cambios-y-eliminaciones-hechas/"
author: milmazz
comments: true
date: 2008-08-12 01:05:42
layout: post
slug: subversion-recuperar-cambios-y-eliminaciones-hechas
title: 'subversion: Recuperar cambios y eliminaciones hechas'
categories:
- subversion
tags:
- Programación
- subversion
- tips
---

Muchos compañeros de trabajo y amigos en general que recién comienzan con el manejo de sistemas de control de versiones centralizados, en particular _subversion_, regularmente tienen inquietudes en cuanto al proceso de recuperación de cambios una vez que han sido enviados al repositorio, así como también la recuperación de ficheros y directorios que fueron eliminados en el pasado. Trataré de explicar algunos casos en base a ejemplos para que se tenga una idea más clara del problema y su respectiva solución.

En el primero de los casos se tiene recuperar la revisión previa a la actual, suponga que usted mantiene un repositorio de recetas, una de ellas en particular es la ensalada _caprese_, por error o descuido añadió el ingrediente _Mostaza tipo Dijón_ a la lista, si usted posee siquiera un lazo con italinos sabe que está cometiendo un error que puede devenir en escarnio público, desprecio e insultos.
    
    ~/svn/wc/trunk$ svn diff -r 2:3 ${URL}/trunk/caprese
    Index: caprese
    ===================================================================
    --- caprese	(revision 2)
    +++ caprese	(revision 3)
    @@ -7,3 +7,4 @@
      - Albahaca fresca
      - Aceite de oliva
      - Pimienta
    + - Mostaza tipo Dijon

Note que el comando anterior muestra las diferencias entre las revisiones 2 y 3 del repositorio, en el resumen se puede apreciar que en la revisión 3 ocurrió el _error_. Un modo rápido de recuperarlo es como sigue.

    ~/svn/wc/trunk$ svn merge -c -3 ${URL}/trunk/caprese
    --- Reverse-merging r3 into 'caprese':
    U    caprese

En este caso particular se están aplicando las diferencias entre las revisiones **consecutivas** a la copia de trabajo. Es hora de verificar que los cambios hechos sean los deseados:
    
    ~/svn/wc/trunk$ svn status
    M      caprese
    ~/svn/wc/trunk$ svn diff
    Index: caprese
    ===================================================================
    --- caprese	(revision 3)
    +++ caprese	(working copy)
    @@ -7,4 +7,3 @@
      - Albahaca fresca
      - Aceite de oliva
      - Pimienta
    - - Mostaza tipo Dijon

Una vez verificado enviamos los cambios hechos al repositorio a través de comando `svn commit`. 

Seguramente usted se estará preguntando ahora que sucede si las revisiones del ficheros no son consecutivas como en el caso mostrado previamente. En este caso es importante hacer notar que la opción `-c 3` es equivalente a `-r 2:3` al usar el comando `svn merge`, en nuestro caso particular `-c -3` es equivalente a `-r 3:2` (a esto se conoce como una fusión reversa), substituyendo la opción `-c` (o `--changes`) en el caso previo obtenemos lo siguiente:

    ~/svn-tests/wc/trunk$ svn merge -r 3:2 ${URL}/trunk/caprese
    --- Reverse-merging r3 into 'caprese':
    U    caprese

**Referencias:** `svn help merge`, `svn help diff`, `svn help status`.

#### Recuperando ficheros o directorios eliminados

Una manera bastante sencilla de recuperar ficheros o directorios eliminados es haciendo uso de comando `svn cp` o `svn copy`, una vez determinada la revisión del fichero o directorio que desea recuperar la tarea es realmente sencilla:

    ~/svn-tests/wc/trunk$ svn cp ${URL}/trunk/panzanella@6 panzanella
    A         panzanella

En este caso se ha duplicado la revisión 6 del fichero `panzanella` en la copia de trabajo local, se ha programado para su adición incluyendo su historial, esto último puede verificarse en detalle al observar el signo **'+'** en la cuarta columna del comando `svn status`.
    
    ~/svn-tests/wc/trunk$ svn status
    A  +   panzanella

**Referencias:** `svn help copy`, `svn help status`.
