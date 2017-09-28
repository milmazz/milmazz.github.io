---
redirect_from: "/archivos/2005/08/08/como-actualizar-de-manera-segura-su-sistema/"
author: milmazz
comments: true
date: 2005-08-08 01:02:32
layout: post
slug: como-actualizar-de-manera-segura-su-sistema
title: COMO actualizar de manera segura su sistema
categories:
- GNU/Linux
- Software
- Ubuntu
tags:
- GNU/Linux
- Software
- Ubuntu
---

Antes de comenzar es importante hacer notar que esta guía se enfocará al mundo [Debian](http://www.debian.org/) GNU/Linux y sus derivadas, en donde por supuesto se incluye [Ubuntu Linux](http://www.ubuntulinux.org/). Después de hacer la breve aclaratoria podemos comenzar.

### ¿Es importante la _firma_ de los paquetes?

La _firma_ de los paquetes es una funcionalidad fundamental para evitar el posible cambio _adrede_ en los ficheros binarios o fuentes distribuidos a través de _sitios espejos_ (_mirrors_), de esta manera nos libramos de la posibilidad de un ataque _man in the middle_, el cual básicamente consiste en la intercepción de la comunicación entre el _origen_ y el _destino_, el atacante puede leer, insertar y modificar los mensajes (en este caso particular, los ficheros) compartidos entre las partes sin que cada una de ellas se percate que la comunicación se ha visto comprometida.

### Nuestro objetivo

Un sistema automatizado de actualización de paquetes, también es sumamente importante **eliminar cualquier posibilidad de amenaza** que pueda surgir al aprovecharse de la automatización del proceso de actualización, por ejemplo, debemos evitar a toda costa la distribución de troyanos que comprometarán la integridad de nuestros sistemas.

### Un poco de historia...

No fue sino hasta la aparición de la versión 0.6 de la interfaz `apt` en donde se realiza la autenticación de ficheros binarios y fuentes de manera transparente haciendo uso de una Infraestructura de clave pública (en inglés, _Public Key Infrastructure_ o PKI). La PKI se basa en el modelo _GNU Privacy Guard_ (GnuPG) y se ofrece un enorme despliegue de _keyservers_ internacionales.

### Detectando la autenticidad de los paquetes

Como se menciono anteriormente desde la versión 0.6 de la interfaz `apt` se maneja de manera transparente el proceso de autentificación de los paquetes. Asi que vamos a hacer una prueba, voy a _simular_ la instalación del paquete `clamav`.

    $ sudo aptitude --simulate install clamav

Obteniendo por respuesta lo siguiente:

    Leyendo lista de paquetes... Hecho
    Creando árbol de dependencias
    Leyendo la información de estado extendido
    Inicializando el estado de los paquetes... Hecho
    Se instalarán automáticamente los siguientes paquetes NUEVOS:
      arj clamav-base clamav-freshclam libclamav1 libgmp3 unzoo
    Se instalarán los siguiente paquetes NUEVOS:
      arj clamav clamav-base clamav-freshclam libclamav1 libgmp3 unzoo
    0 paquetes actualizados, 7 nuevos instalados, 0 para eliminar y 0 sin actualizar.
    Necesito descargar 3248kB de ficheros. Después de desempaquetar se usarán 4193kB.
    ¿Quiere continuar? [Y/n/?] Y
    The following packages are not AUTHENTICATED:
      clamav clamav-freshclam clamav-base libclamav1
    
    Do you want to continue? [y/N] N
    Cancela.

Si nos fijamos bien en la respuesta anterior notaremos que ciertos paquetes no han podido ser autentificados. A partir de este punto _es responsabilidad del administrador del sistema el instalar o no dichos paquetes_, por supuesto, cada quien es responsable de sus acciones, yo prefiero declinar mi intento por el momento y asegurarme de la autenticidad de los paquetes, para luego proceder con la instalación normal.

### Comienza la diversión

Ahora bien, vamos a mostrar la secuencia de comandos a seguir para agregar las llaves públicas dentro del _keyring_ por defecto. Antes de entrar en detalle es importante aclarar que el ejemplo agregará la llave pública del grupo [os-cillation](http://www.os-cillation.de), quienes se encargan de mantener paquetes para el entorno de escritorio [Xfce](http://www.xfce.org/) (siempre actualizados y manera no-oficial) para la distribución [Debian](http://www.debian.org/) GNU/Linux (también sirven para sus derivadas, como por ejemplo [Ubuntu Linux](http://www.ubuntulinux.org/)).

#### Importando la llave pública desde un servidor GPG

    $ gpg --keyserver hkp://wwwkeys.eu.pgp.net --recv-keys 8AC2C0A6

El comando anterior simplemente importara la llave especificada (`8AC2C0A6`) desde el servidor con el cual se ha establecido la comunicación, el valor de la opción `--keyserver` sigue cierto formato, el cual es: `esquema:[//]nombreservidor[:puerto]`, los valores ubicados entre corchetes son _opcionales_, cuando hablamos del _esquema_ nos referimos al tipo de servidor, regularmente utilizaremos como esquema `hkp` para servidores HTTP o compatibles.

Si el comando anterior se ejecuto de manera correcta, el proceso nos arrojará una salida similar a la siguiente:

    gpg: key 8AC2C0A6: public key "os-cillation Debian Package Repository 
    (Xfld Package Maintainer) <debian-packages@os-cillation.com>" imported

La instrucción anterior solamente variará de acuerdo al `keyserver` y la  clave que deseemos importar. En [www.pgp.net](http://www.pgp.net/) está disponible un buscador que le facilitará la obtención de los datos necesarios.

#### Exportando y añadiendo la llave pública

    $ gpg --armor --export 8AC2C0A6 | sudo apt-key add -

Con el comando anterior procedemos a construir la salida en formato ASCII producto de la exportación de la llave especificada y a través del [pipe](http://www.faqs.org/docs/bashman/bashref_17.html) capturamos la salida estándar y la utilizamos como entrada estándar en el comando `apt-key add`, el cual simplemente agregará una nueva llave a la lista de llaves confiables, dicha lista puede visualizarse al hacer uso del comando `apt-key list`.

Aunque parezca evidente la aclaratoria, recuerde que si usted no puede hacer uso de [sudo](/archivos/2005/05/03/es-necesario-activar-la-cuenta-root-en-ubuntu/), debe identificarse previamente como _superusuario_.

#### Finalmente...

Para finalizar recuerde que debe actualizar la lista de paquetes.

    $ sudo aptitude update

Ahora podemos proceder a instalar los paquetes desde el repositorio que hemos añadido como fuente confiable.
