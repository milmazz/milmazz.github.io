---
redirect_from: "/archivos/2006/01/27/integridad-del-cd-de-ubuntu/"
author: milmazz
comments: true
date: 2006-01-27 20:38:47
layout: post
slug: integridad-del-cd-de-ubuntu
title: Integridad del CD de Ubuntu
categories:
- Ubuntu
tags:
- linux
- Ubuntu
---

Recientemente un amigo (de ahora en adelante lo llamaré _pepito_)  al que le regalé un par de [CD's de Ubuntu](https://shipit.ubuntu.com/), me preguntó después de unos días lo siguiente: Milton, ¿cómo puedo verificar la integridad del CD de instalación de Ubuntu?.

En primer lugar, estaba muy contento porque _pepito_ deseaba en realidad **migrar** a GNU/Linux. Pero la idea de este artículo no es hablarles de _pepito_, sino describir lo más detalladamente posible la respuesta que le dí.

## Aprovechar las opciones que nos brinda el CD de instalación

El mismo CD de instalación de ubuntu nos brinda una opción que nos permite verificar la integridad del disco, para ello debemos realizar lo siguiente:

  1. Colocar el CD de instalación de Ubuntu en la unidad de CD-ROM correspondiente, seguidamente proceder a reiniciar, recuerde que en la configuración de la BIOS debe tener como primera opción de arranque la unidad de CD-ROM que corresponda en su caso.
  2. Al terminar la carga del CD usted podrá apreciar un mensaje de bienvenida similar al siguiente:

> The default installation is suitable for most desktop or laptops systems. Press F1 for help and advanced installation options.
>
> To install only the base system, type "server" then ENTER. For the default installation, press ENTER.
>
> boot:_

Lo anterior, traducido a nuestro idioma sería similar a:

> La instalación por defecto es conveniente para la mayoría de los sistemas de escritorio o portátiles. Presione F1 para ayuda y opciones de instalación avanzadas.

>
> Para instalar solo el sistema base, escriba "server" luego ENTER. Para la instalación por defecto, presione ENTER.

  3. Para este artículo, se realizará el modo de instalación por defecto, lo anterior quiere decir que solamente debemos presionar la tecla ENTER, enseguida observaremos la carga del _kernel_.
  4. Desde el cuadro de dialogo _Choose Language_, primero en aparecer, presionaremos la tecla Tab y seguidamente debemos seleccionar la opción _Go back_
  5. El paso anterior nos llevará al menú principal de la instalación de Ubuntu (_Ubuntu installer main menu_), una vez ubicados acá, simplemente debemos seleccionar la opción _Check the CD-ROM(s) Integrity_.
  6. Al finalizar el paso anterior nos llevará a un cuadro de dialogo de confirmación, pero antes podremos notar una pequeña advertencia:

> **Warning:** this check depends on your hardware and may take some time.
>
> Check CD-ROM integrity?

Lo anterior, traducido a nuestro idioma sería similar a:

> **Advertencia:** Esta revisión depende de su hardware y puede tomar cierto tiempo.
>
> Revisar la integridad del CD-ROM?

A la pregunta anterior respondemos **Sí**

  7. Si lo prefiere, salga de su casa, tome un poco de sol y regrese ;)
  8. Si el CD-ROM no tiene fallo alguno, podrá observar un mensaje al final de la revisión similar al siguiente:

> Integrity test successful the CD-ROM. Integrity test was successful.

**The CD-ROM is valid**

Si la revisión de la integridad del CD-ROM _es satisfactoría_, puede continuar con el proceso de instalación.

Suerte y bienvenido al mundo GNU/Linux ;)
