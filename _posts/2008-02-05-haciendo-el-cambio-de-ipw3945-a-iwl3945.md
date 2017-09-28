---
redirect_from: "/archivos/2008/02/05/haciendo-el-cambio-de-ipw3945-a-iwl3945/"
author: milmazz
comments: true
date: 2008-02-05 05:35:01
layout: post
slug: haciendo-el-cambio-de-ipw3945-a-iwl3945
title: Haciendo el cambio de ipw3945 a iwl3945
categories:
- debian
tags:
- debian
- intel
- ipw3945
- iwl3945
- wireless
---

Si usted es de esas personas que cuenta con una tarjeta inalámbrica _Intel Corporation PRO/Wireless 3945_, seguramente sabrá que existen al menos dos proyectos que le dan soporte. El primero de ellos es [ipw3945](http://ipw3945.sourceforge.net/) y se encuentra **obsoleto**, el desarrollo pasó al proyecto [iwlwifi](http://intellinuxwireless.org/).

Aprovechando que recientemente ha ingresado a la versión inestable de [Debian](http://www.debian.org/) la serie del kernel `2.6.24`, este contiene el nuevo modulo `iwl3945` que reemplaza al viejo `ipw3945`. Una de las ventajas de este cambio es que ya no hay necesidad de tener activo el demonio `ipw3945d`. Sin embargo, aun se necesita del _firmware_ que se encuentra en la sección `non-free` del repositorio de [Debian](http://www.debian.org/).

Hasta donde he leído el plan será remover los paquetes `ipw3945-modules-*` e `ipw3945d` de los repositorios de Debian (al menos en _testing_ y en _unstable_) una vez que la serie `2.6.24` del kernel llegue a la versión de pruebas (_testing_). Aquellos que se encuentren hoy día en la versión inestable (_unstable_) de Debian deberán cambiar el driver desde `ipw3945` a `iwl3945`. Para aquellos que trabajan en _etch_ también es posible usar el driver `iwl3945` si actualiza su versión del kernel por medio del repositorio [etch-backports](http://backports.org/) (el nuevo _stack_ `mac80211` que usa `iwlwifi` se encuentra a partir de la versión del kernel `2.6.22`).

Las instrucciones que verá a continuación se han aplicado en _Debian inestable_, si usted desea instalar `iwlwifi` en etch puede seguir estas [instrucciones](http://nanonanonano.net/linux/debian/iwlwifi).

Obteniendo algunos datos de interés antes de proceder con la actualización.

Versión del kernel:

    $ uname -r
    2.6.22-3-686

Verifique que en realidad tiene una tarjeta _Intel Corporation PRO/Wireless 3945_
    $ lspci -nn | grep Wireless
    03:00.0 Network controller [0280]: Intel Corporation PRO/Wireless 3945ABG Network Connection [8086:4227] (rev 02)

## Paquetes necesarios

Ahora bien, es necesario instalar el nuevo _kernel_ y el _firmware_ necesario para hacer funcionar a `iwlwifi`

    # aptitude install linux-image-2.6-686 \\
    linux-image-2.6.24-1-686 \\
    firmware-iwlwifi

## Evitando problemas

Verifique que no existe alguna entrada que haga referencia al modulo `ipw3945` en el fichero `/etc/modules`. Para ello recurrimos a **Perl** que nos facilita la vida.

    # perl -i -ne 'print unless /^ipw3945/' /etc/modules

Debido a algunos problemas que se presentan en el paquete [network-manager](http://packages.debian.org/network-manager) si anteriormente ha venido usando el modulo `ipw3945` se recomienda **eliminar** la entrada que genera `udev` para dicho modulo en el fichero `/etc/udev/rules.d/z25_persistent-net.rules`, la entrada es similar a la siguiente:
    
    # PCI device 0x8086:0x4227 (ipw3945)
    SUBSYSTEM=="net", DRIVERS=="?*", ATTRS{address}=="00:13:02:4c:12:12", NAME="eth2"

## Fichero /etc/network/interfaces

Este paso es opcional, agregamos la nueva interfaz `wlan0` al fichero `/etc/network/interfaces` y procedemos a configurarla de acuerdo a nuestras necesidades.
    
    auto lo
    iface lo inet loopback
    
    auto wlan0
    iface wlan0 inet dhcp
    wpa-driver wext
    wpa-ssid foo
    wpa-psk baz
    wpa-key-mgmt WPA-PSK
    wpa-pairwise TKIP CCMP
    wpa-group TKIP CCMP
    wpa-proto WPA RSN

En este caso particular se está indicando que nos vamos a conectar a un _Access Point_ cuyo `ssid` es `foo` con tipo de cifrado WPA-PSK/WPA2-PSK, haciendo uso del driver `wext` que funciona como _backend_ para `wpa_supplicant`. Es de hacer notar que el driver `wext` es utilizado por todos los adaptadores _Intel Pro Wireless_, eso incluye `ipw2100`, `ipw2200` e `ipw3945`.

Para hacer funcionar **WPA** recuerde que debe haber instalado previamente el paquete `wpasupplicant`.
    
    # aptitude install wpasupplicant

De igual manera se le recuerda adaptar todos aquellos parámetros como `wpa-ssid` y `wpa-psk` a aquellos adecuados en su caso. En particular el campo `wpa-psk` lo puede generar con el siguiente comando:
    
    $ wpa_passphrase su_ssid su_passphrase

Aunque mi recomendación es usar el comando `wpa_passphrase` de la siguiente manera.
    
    $ wpa_passphrase su_ssid

Posteriormente deberá introducir `su_passphrase` desde la entrada estándar, esto evitará que `su_passphrase` quede en el historial de comandos.

Para mayor detalle de los campos expuestos en la configuración del fichero `/etc/network/interfaces` se le recomienda leer la documentación expuesta en `/usr/share/doc/wpasupplicant/README.modes.gz`.

Una vez concluidos estos pasos reiniciamos el sistema y seleccionamos en nuestro _Gestor de Arranque_ (ej. GRUB) la versión del _kernel_ recien instalada. Al momento de iniciar su sesión verifique que su tarjeta inalámbrica esté funcionando, de lo contrario haga las revisiones que se indican en la siguiente sección.

## En caso de persistir los problemas

Remueva y reinserte el modulo `iwl3945`

    # modprobe -r iwl3945
    # modprobe iwl3945

De manera adicional compruebe que `udev` haya generado una nueva entrada para `iwl3945`.
    
    $ cat /etc/udev/rules.d/z25_persistent-net.rules
    ...
    # PCI device 0x8086:0x4227 (iwl3945)
    SUBSYSTEM=="net", DRIVERS=="?*", ATTR{address}=="00:13:02:4c:12:12", ATTR{type}=="1", NAME="wlan0"

Finalmente, reestablecemos la interfaz de red.
    
    # ifdown wlan0
    # ifup wlan0

## Elimine ipw3945

Una vez verificado el correcto funcionamiento del módulo `iwl3945` puede eliminar con seguridad todo aquello relacionado con el modulos `ipw3945`.
    
    # aptitude --purge remove firmware-ipw3945 \\
    ipw3945-modules-$(uname -r) \\
    ipw3945-source ipw3945d

Estas instrucciones también aplican para el modulo `iwl4965`. Mayor información en [Debian Wiki § iwlwifi](http://wiki.debian.org/iwlwifi).
