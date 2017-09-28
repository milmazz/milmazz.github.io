---
redirect_from: "/archivos/2007/04/21/establecer-red-inalambrica-en-dell-m1210/"
author: milmazz
comments: true
date: 2007-04-21 22:05:36
layout: post
slug: establecer-red-inalambrica-en-dell-m1210
title: Establecer red inalámbrica en Dell m1210
categories:
- debian
- GNU/Linux
tags:
- debian
- dell
- GNU/Linux
- m1210
- ndiswrapper
---

Hace ya algunos días [Ana](http://an1ta.wordpress.com) me comentaba que no le estaba funcionando la configuración que tenía para su red inalámbrica, eso ocurrió una vez que actualizó la versión del _kernel_ de _linux_, espero entrar en detalle acerca de los pasos que seguí para configurarle todo como se debe bajo [Debian Etch](http://www.us.debian.org/releases/etch/).

Lo primero que debía saber era el tipo de componente PCI al que me estaba enfrentando.
    
    $ lspci -nn | grep Wireless
    0c:00.0 Network controller [0280]: 
    Broadcom Corporation Dell Wireless 1390 
    WLAN Mini-PCI Card [14e4:4311] (rev 01)

Lo anterior dice que nos estamos enfrentando ante una **Broadcom** cuyo **chipset id** es el _4311_, debemos saber que el módulo para _linux_ de estos _chips_ es el `bcm43xx` y ha sido incluido al _kernel_ de _linux_ desde la versión **2.6.17-rc2**Fuente: [http://bcm43xx.berlios.de/](http://bcm43xx.berlios.de/), al revisar la [lista de dispositivos soportados](http://bcm43xx.berlios.de/?go=devices) me percaté que el soporte para este **chipset id** aún es _inestable_, así que el siguiente paso era eliminar su presencia si aplicaba.

    $ lsmod | grep bcm43xx
    bcm43xx               148500  0
    ieee80211softmac       40704  1 bcm43xx
    ieee80211              39112  2 bcm43xx,ieee80211softmac

Como se puede observar en este caso aplica, así que comenzamos a eliminar su presencia.
    
    # grep -q '^blacklist bcm43xx' /etc/modprobe.d/blacklist \\
    || tee -a 'blacklist bcm43xx' /etc/modprobe.d/blacklist

La inclusión de la línea `blacklist bcm43xx` al fichero `/etc/modprobe.d/blacklist` si aplica me permite indicar que dicho módulo no debe cargarse como resultado de la expansión de su alias, es decir, `bcm43xx`, esto se hace con el propósito de evitar que el subsistema _hotplug_ lo carge, aunque esto no evita que el módulo se carge automáticamente por el _kernel_.

Luego verifique el fichero `/etc/modules`, el cual contiene los nombre de los módulos que serán cargados a la hora del inicio del sistema, no había entrada para el módulo `bcm43xx`, ahora es necesario remover dicho módulo, para lo cual hacemos:
    
    # modprobe -r bcm43xx

Una vez culminado este proceso es necesario hacer uso de [ndiswrapper](http://ndiswrapper.sourceforge.net/), el cual es un módulo que me permite cargar y ejecutar _drivers_ propietarios de Windows para tarjetas inalámbricas.
    
    # aptitude -r install build-essential \\
    module-assistant ndiswrapper-common
    # m-a update
    # m-a prepare
    # m-a a-i ndiswrapper
    # modprobe ndiswrapper

Una vez cargado el módulo `ndiswrapper` es necesario instalar el nuevo _driver_ propietario, para ello debemos encontrar el fichero con extensión **inf**, este fichero especifica que ficheros necesitan estar presentes o descargarse para que el componente funcione correctamente, para dicho _driver_. Al consultar en la [lista de tarjetas que funcionan con ndiswrapper](http://ndiswrapper.sourceforge.net/mediawiki/index.php/List) me percato que han habido problemas de seguridad en algunos de los _drivers_ recomendados para esta tarjeta, así que para asegurarme de obtener las versiones más recientes ingreso al sitio oficial de [Dell](http://www.dell.com), bajo la sección _USA_ -> _Support search: "m1210"_ -> _Drivers and Downloads_ -> _Network & Internet_ -> _Network Driver_, ingreso el campo correspondiente al _service tag_, y finalmente descargo el fichero **R151517.EXE**.

El siguiente paso es extraer los ficheros que se encuentran dentro de **R151517.EXE**, para ello:
    
    unzip R151517.EXE

Ahora nos interesa el fichero `bcmwl5.inf` que está dentro del directorio `DRIVER`.

    $ tree R151517/DRIVER/
    R151517/DRIVER/
    |-- bcm43xx.cat
    |-- bcm43xx64.cat
    |-- bcmwl5.inf
    |-- bcmwl5.sys
    `-- bcmwl564.sys

Una vez extraídos los ficheros, procedemos a cargar el _driver_, para ello hacemos lo siguiente:

    # ndiswrapper -i R151517/DRIVER/bcmwl5.inf

Comprobamos que el _driver_ se ha instalado correctamente.
    
    # ndiswrapper -l
    installed drivers:
    bcmwl5          driver installed, hardware (14E4:4324) present (alternate driver: bcm43xx)

Luego verificamos nuestro trabajo al ejecutar el comando `dmesg`, tal como se muestra a continuación:

    $ dmesg
    [44093.473325] ndiswrapper version 1.27 loaded (preempt=no,smp=yes)
    [44095.311236] ndiswrapper (link_pe_images:577): fixing KI_USER_SHARED_DATA address in the driver
    [44093.482777] ndiswrapper: driver bcmwl5 (Broadcom,03/23/2006, 4.40.19.0) loaded
    [44093.483250] ACPI: PCI Interrupt 0000:0c:00.0[A] -> GSI 17 (level, low) -> IRQ 177
    [44093.483367] PCI: Setting latency timer of device 0000:0c:00.0 to 64
    [44093.491760] ndiswrapper: using IRQ 177
    [44094.162703] wlan0: vendor: 
    [44094.162708] wlan0: ethernet device 00:18:f3:6b:fc:3b using NDIS driver bcmwl5, 14E4:4311.5.conf
    [44094.162772] wlan0: encryption modes supported: WEP; TKIP with WPA, WPA2, WPA2PSK; AES/CCMP with WPA, WPA2, WPA2PSK
    [44094.166554] usbcore: registered new driver ndiswrapper
    [44094.167390] ndiswrapper: changing interface name from 'wlan0' to 'eth1'

En este preciso instante el comando `ifconfig -a` debe mostrarnos la nueva interfaz, y el comando `iwlist eth1 scan` al ejecutarse como _superusuario_ devolverá la lista de redes que han sido detectadas.

Recuerde que para que todo esto siga funcionando aún después de reiniciar el sistema, es necesario cargar el módulo de `ndiswrapper`, para ello hago uso del comando [modconf](http://www.debian.org/doc/manuals/reference/ch-system.en.html#s-modules).
  *[PCI]: Peripheral Component Interconnect
