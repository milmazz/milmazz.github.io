---
title: Conozca la temperatura de su disco duro
author: milmazz
date: 2005-09-21 00:13:30
categories:
  - GNU/Linux
  - Ubuntu
tags:
  - GNU/Linux
  - Recursos
  - Ubuntu
slug: conozca-la-temperatura-de-su-disco-duro
redirect_from: /archivos/2005/09/21/conozca-la-temperatura-de-su-disco-duro/
---

Si desea conocer cual es el valor en grados centígrados de la temperatura de su
disco duro, simplemente instale el paquete `hddtemp` desde el repositorio
`universe` haciendo lo siguiente:

    $ sudo aptitude install hddtemp

Después, siempre que desee conocer la temperatura actual de su disco duro,
proceda de la siguiente manera:

    $ sudo hddtemp /dev/hdb

Por supuesto, recuerde que en la línea anterior `/dev/hdb` es el identificador
de mi segundo disco duro, proceda a cambiarlo si es necesario.

Mi temperatura actual en el segundo disco duro es de:

    milmazz@omega:~$ sudo hddtemp /dev/hdb
    /dev/hdb: ST340014A: 46°C

Antes de finalizar, es importante resaltar que `hddtemp` le mostrará la
temperatura de su disco duro IDE o SCSI **solamente** si éstos soportan la
tecnología SMART (acrónimo de: _Self-Monitoring, Analysis and Reporting
Technology_).

SMART simplemente es una tecnología que de manera automática realiza un
monitoreo, análisis e informes, ésta tiene un sistema de alarma que en la
actualidad viene de manera predeterminada en muchos modelos de discos duros, lo
anterior puede ayudarle a evitar fallas que de una u otra manera pueden
afectarle de manera contundente.

En esencia, SMART realiza un monitoreo del comportamiento del disco duro y si
éste presenta un comportamiento poco común, será analizado y reportado al
usuario.

**Vía:** [Ubuntu Blog](http://ubuntu.wordpress.com/2005/09/18/find-the-temperature-of-your-hard-drive/).
