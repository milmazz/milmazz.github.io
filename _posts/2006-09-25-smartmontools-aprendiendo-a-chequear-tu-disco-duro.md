---
redirect_from: "/archivos/2006/09/25/smartmontools-aprendiendo-a-chequear-tu-disco-duro/"
author: chantanito
comments: true
date: 2006-09-25 01:39:50
layout: post
slug: smartmontools-aprendiendo-a-chequear-tu-disco-duro
title: 'Smartmontools: aprendiendo a chequear tu disco duro...'
wordpress_id: 199
categories:
- debian
- GNU/Linux
tags:
- debian
- GNU/Linux
---

Los discos duros modernos (y no tan modernos) vienen equipados con una tecnología conocida como S.M.A.R.T., el cual le permite al disco monitorear de manera contínua su propio estado de "salud" y alertar al usuario si es detectada alguna anormalidad, para que luego pueda ser corregida.

**ADVERTENCIA:** antes de continuar, sería recomendable _hacer una copia de respaldo de todos sus datos importantes_ a pesar de todo lo que diga el S.M.A.R.T. Éste sistema es muy confiable pero no obstante, ésta información en alguno de los casos podría ser imprecisa, de hecho, los discos duro se dañan de manera inesperada, inclusive si el S.M.A.R.T te ha dicho que algo anda mal, posiblemente no tengas tiempo para respaldar tus datos o moverlos a un lugar más seguro.

## ¿Cómo instalar SMARTMONTOOLS?

Lo primero que debemos hacer, preferiblemente antes de instalar el SMT es chequear si nuestro disco duro soporta éste tipo de tecnología, lo cual puedes hacer visitando la página del fabricante de tu disco. De todas formas, si lo compraste después del año 1992, lo más seguro es que posea ésta tecnología.

Lo segundo, y no menos importante, es **activar o asegurarte que en el BIOS de la tarjeta madre este activada ésta función**. Lo puedes conseguir ya que luce algo como:

     S.M.A.R.T for Hard Disk:	Enable

Algunos BIOS no tienen ésta opción y reportan el S.M.A.R.T como inactivo, pero no te preocupes que el _smartcl_, uno de los dos programas de utilidad que tiene el Smartmontools, puede activarlo. Una vez que estemos seguros de todo ésto podemos proceder a instalar el Smartmontools, el cual, en la distribución que uso, [Debian](http://www.debian.org), es tan fácil como escribir en una terminal:

     $ aptitude install smartmontools

Y de esa manera ya queda automágicamente instalado el paquete el sistema. Si quieres verificar si ya lo tienes instalado, entonces tendrías que escribir en una terminal lo siguiente:

     $ aptitude show smartmontools

Y verificar que el atributo _Estado_ se corresponda con _Instalado_. Una vez hecho ésto procedemos a verificar si nuestro disco soporta S.M.A.R.T con la siguiente línea de comandos:

    $ smartctl -i /dev/hda

En caso que tu disco sea SATA tendrías que escribir la siguiente línea:

    $ smartctl -i -d ata /dev/sda

La información se vería algo así:

    === START OF INFORMATION SECTION ===
    Model Family:     Western Digital Caviar family
    Device Model:     WDC WD1200BB-00RDA0
    Serial Number:    WD-WMANM1700779
    Firmware Version: 20.00K20
    User Capacity:    120,034,123,776 bytes
    Device is:        In smartctl database [for details use: -P show]
    ATA Version is:   7
    ATA Standard is:  Exact ATA specification draft version not indicated
    Local Time is:    Sun Sep 24 22:27:09 2006 VET
    SMART support is: Available - device has SMART capability.
    SMART support is: Enabled
    
Si tu disco es SATA pero tiene el soporte S.M.A.R.T  desactivado, entonces deberás usar la siguiente línea de comandos para activarlo:

    $ smartctl -s on -d ata /dev/sda

## Aprendiendo a usar SMT

### Estado de "salud" de nuestro disco duro

Para leer la información que SMT ha recopilado acerca de nuestro disco, debemos escribir la siguiente linea de comandos:

    $ smartctl -H /dev/hda

Para discos SATA no es suficiente con sustituir _hda_ con _sda_, sino que debemos añadir las opciones extras que usamos anteriormente para obtener la información del disco. La línea de comandos entonces quedaría así:

    $ smartctl -d ata -H /dev/sda

Si lees **PASSED** al final de la información, no tienes que preocuparte. Pero si lees **FAILED** deberías empezar a hacer respaldo de tus datos inmediatamente, ya que ésto quiere decir que tu disco ha presentado fallas anteriormente o es muy probable que falle dentro de un lapso de aproximadamente 24 horas.

### Bitácora de errores de SMT

Para chequear la bitácora de errores de SMT debemos escribir la siguiente línea de comandos:

    $ smartctl -l error /dev/hda

De manera análoga al caso anterior, añadimos los comandos extras si nuestro disco es SATA. La línea de comandos quedaría así:

    $ smartctl -d ata -l error /dev/sda

Si leemos al final de la información **No errors logged** todo anda bien. Si hay varios errores pero éstos no son muy recientes, deberías empezar a preocuparte e ir comprando los discos para el respaldo. Pero si en vez de ésto hay muchos errores y una buena cantidad son recientes entonces deberías empezar a hacer respaldo de tus datos, inclusive antes de terminar de leer ésta línea. Apurate! :D

A pesar que smartctl nos da una información muy valiosa acerca de nuestros discos, el tan solo revisar ésta no es suficiente, realmente se deben hacer algunas pruebas específicas para corroborar los errores que conseguimos en la información anterior, dichas pruebas las menciono a continuación.

### Pruebas con SMT

Las pruebas que se van a realizar a continuación no interfieren con el funcionamiento normal del disco y por lo tanto pueden ser realizadas en cualquier momento. Aquí solo describiré como ejecutarlas y entender los errores. Si quieres saber más te recomiendo que visites [ésta pagina](http://www.linuxjournal.com/article/6983) o que leas las páginas del manual.

Lo primero sería saber cuáles pruebas soporta tu disco, lo cual logramos mediante la siguiente línea de comandos:

    $ smartctl -c /dev/hda

De ésta manera puedes saber cuáles pruebas soporta tu disco y también cuanto tiempo aproximadamente puede durar cada una. Bien, ahora ejecutemos _Immediate Offline Test_ ó Prueba Inmediata Desconetado (si está soportada, por supuesto), con la siguiente línea de comandos:

    $ smartctl -t offline /dev/hda

Como ya sabemos, los resultados de ésta prueba no son inmediatos, de hecho el resultado de ésta linea de comandos te dirá que la prueba ha comenzado, el tiempo que aproximadamente va a demorar la prueba en terminar con una hora aproximada para dicha finalización, y al final te dice la línea de comandos para abortar la operación. En uno de mis discos, un IDE de 120Gb, demoró 1 hora, para que tengan una idea. Por los momentos sólo te queda esperar por los resultados. Ahora te preguntarás _¿Cómo puedo saber los resultados?_, pues tan sencillo como ejecutar la línea de comandos para leer la bitácora del SMT y seguir las recomendaciones.

Ahora vamos a ejecutar _Short self-test routine_ ó _Extended self-test routine_, Rutina corta de autoprueba y Rutina extendida de autoprueba, respectivamente (de nuevo, si están soportadas). Éstas pruebas son similares, sólo que la segunda, como su nombre lo indica, es más rigurosa que la primera. Una vez más ésto lo logramos con los siguientes comandos:

    $ smartctl -t short /dev/hda
    $ smartctl -t long /dev/hda

Luego vamos a chequear el _Self Test Error Log_ ó Bitácora de errores de autopruebas:

    $ smartctl -l selftest /dev/hda

Luego ejecutamos _Conveyance Self Test_ ó Autoprueba de Transporte:

    $ smartctl -t conveyance /dev/hda

Y por último chequeamos _Self Test Error Log_ de nuevo.

Algo que tengo que resaltar es que el SMT tiene dos bitácoras, una de las cuales es la Bitácora de errores y la otra es la Bitácora de Errores de Autopruebas.

## ¿Cómo monitorear el disco duro automáticamente?

Para lograr ésto tendrás que configurar el demonio _smartd_ para que sea cargado cuando se inicia el sistema. A continuación un pequeño HOWTO.

**ADVERTENCIA:** el siguiente HOWTO es para monitorear un disco IDE, programar todas las pruebas cada viernes de la semana, de 11:00 a.m a 2:00 p.m, y luego ejecutar un script de Bash si algún error es detectado. Éste script escribirá un reporte bien detallado y luego apagará el equipo para su propia protección.

El archivo de configuración de _smartd_ se encuentra en `/etc/smartd.conf`, si éste no existe,
el cual sería un caso poco común, deberás crearlo en tu editor de elección.

    ...
    #DEVICESCAN
    ...
    /dev/hda \ 
    -H \
    -l error -l selftest \
    -s (O/../../5/11|L/../../5/13|C/../../5/15) \
    -m ThisIsNotUsed -M exec /usr/local/bin/smartd.sh

El script en Bash también puedes hacerlo en un editor de tu elección, y tendrá la siguiente forma:

    #!/bin/bash
    LOGFILE="/var/log/smartd.log"
    echo -e "$(date)\n$SMARTD_MESSAGE\n" >> "$LOGFILE"
    shutdown -h now

Luego de crear el script deberás hacerlo ejecutable cambiando sus atributos con la siguiente línea de comandos:

    $ chmod +x /usr/local/bin/smartd.sh

Para probar todo, puedes agregar al final de tu `/etc/smartd.conf` la línea
    
    -M test

y luego ejecutar el demonio. Nota que ésto apagará tu equipo, hayan errores o no. Luego si algo sale mal deberás chequear la bitácora del sistema en /var/log/messages con:

    $ tail /var/log/messages

Y así corregir el problema. Ahora tendrás que quitar la línea 
    
    -M test

y guardar los cambios. Por último debes hacer que _smartd_ sea cargado al momento del arranque, lo cual lo logras con la siguiente línea:

    $ rc-update add smartd default

Algunos enlaces de interés:

  * [S.M.A.R.T en la Wikipedia](http://es.wikipedia.org/wiki/SMART)
  * [Página Oficial de smartmontools](http://smartmontools.sourceforge.net/)
  * [Ejemplos de salidas del SMT](http://smartmontools.sourceforge.net/#sampleoutput)
  * [Atributos usados para recuperar el estado físico/lógico de una unidad y mostrar su significado de una forma más entendible para un usuario final](http://smartlinux.sourceforge.net/smart/attributes.php)

Nota: Ésta artículo está basado en un [HOWTO](http://gentoo-wiki.com/HOWTO_Monitor_your_hard_disk(s)_with_smartmontools) del [Wiki de Gentoo](http://gentoo-wiki.com/Main_Page)
