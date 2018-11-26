---
redirect_from: "/archivos/2008/09/20/configurando-nuestras-interfaces-de-red-con-ifupdown/"
author: milmazz
comments: true
date: 2008-09-20 02:34:32
layout: post
slug: configurando-nuestras-interfaces-de-red-con-ifupdown
title: Configurando nuestras interfaces de red con ifupdown
categories:
- GNU/Linux
tags:
- debian
- GNU/Linux
- ifdown
- ifup
- Ubuntu
- wireless
---

Si usted es de esas personas que suele mover su máquina portátil entre varias redes que no necesariamente proveen [DHCP](http://es.wikipedia.org/wiki/DHCP) y usualmente vuelve a configurar sus preferencias de conexión, seguramente este artículo llame su atención puesto que se explicará acerca de la configuración de diversos _perfiles de conexión_ vía línea de comandos.

En los sistemas [Debian](http://www.debian.org) y los basados en él, [Ubuntu](http://www.ubuntu.com) por ejemplo, para lograr la configuración de las redes existe una herramienta de alto nivel que consiste en los comandos `ifup` e `ifdown`, adicionalmente se cuenta con el fichero de configuración `/etc/network/interfaces`. También el paquete `wireless-tools` incluye un script en `/etc/network/if-pre-up.d/wireless-tools` que hace posible preparar el hardware de la interfaz inalámbrica antes de darla de alta, dicha configuración se hace a través del comando `iwconfig`.

Para hacer uso de las ventajas que nos ofrece la herramienta de alto nivel `ifupdown`, en primer lugar debemos editar el fichero `/etc/network/interfaces` y establecer nuestros _perfiles_ de la siguiente manera:

    auto lo
    iface lo inet loopback

    # Conexión en casa usando WPA
    iface home inet dhcp
        wpa-driver wext
        wpa-ssid foo
        wpa-psk baz
        wpa-keymgmt WPA-PSK
        wpa-pairwise TKIP CCMP
        wpa-group TKIP CCMP
        wpa-proto WPA RSN

    # Conexión en la oficina
    # sin DHCP
    iface office inet static
        wireless-essid bar
        wireless-key s:egg
        address 192.168.1.97
        netmask 255.255.255.0
        broadcast 192.168.1.255
        gateway 192.168.1.1
        dns-search company.com #search@resolv.conf
        dns-nameservers 192.168.1.2 192.168.1.3 #nameserver@resolv.conf

    # Conexión en reuniones
    iface meeting inet dhcp
    	wireless-essid ham
    	wireless-key s:jam

En este ejemplo se encuentran 3 configuraciones particulares (`home`, `work` y `meeting`), la primera de ellas define que nos vamos a conectar con un _Access Point_ cuyo _ssid_ es `foo` con un tipo de cifrado WPA-PSK/WPA2-PSK, esto fue explicado en detalle en el artículo [Haciendo el cambio de ipw3945 a iwl3945](/article/2008/02/05/haciendo-el-cambio-de-ipw3945-a-iwl3945). La segunda configuración indica que nos vamos a conectar a un _Access Point_ con una IP estática y configuramos los parámetros `search` y `nameserver` del fichero `/etc/resolv.conf` (para más detalle lea la documentación del paquete `resolvconf`). Finalmente se define una configuración similar a la anterior, pero en este caso haciendo uso de DHCP.

Llegados a este punto es importante aclarar lo que `ifupdown` considera una _interfaz lógica_ y una _interfaz física_. La **interfaz lógica** es un valor que puede ser asignado a los parámetros de una interfaz física, en nuestro caso `home`, `office`, `meeting`. Mientras que la **interfaz física** es lo que propiamente conocemos como la interfaz, en otras palabras, lo que regularmente el _kernel_ reconoce como `eth0`, `wlan0`, `ath0`, `ppp0`, entre otros.

Como puede verse en el ejemplo previo las definiciones adyacentes a `iface` hacen referencia a **interfaces lógicas**, no a interfaces físicas.

Ahora bien, para dar de alta la _interfaz física_ `wlan0` haciendo uso de la _interfaz lógica_ `home`, como superusuario puede hacer lo siguiente:

    # ifup wlan0=home

Si usted ahora necesita reconfigurar la _interfaz física_ `wlan0`, pero en este caso particular haciendo uso de la _interfaz lógica_ `work`, primero debe dar de baja la interfaz física `wlan0` de la siguiente manera:

    # ifdown wlan0

Seguidamente deberá ejecutar el siguiente comando:

    # ifup wlan0=work

Es importante hacer notar que tal como está definido ahora el fichero `/etc/network/interfaces` ya no es posible dar de alta la interfaz física `wlan0` ejecutando solamente lo siguiente:

    ifup wlan0

La razón de este comportamiento es que el comando `ifup` utiliza el nombre de la _interfaz física_ como el nombre de la _interfaz lógica_ por omisión y evidentemente ahora **no** está definido en el ejemplo un nombre de interfaz lógica igual a `wlan0`.

En un próximo artículo se harán mejoras en la definición del fichero `/etc/network/interfaces` y su respectiva integración con una herramienta para la detección de redes que tome como entrada una lista de perfiles de redes candidatas, cada una de ellas incluyendo casos de pruebas. Teniendo esto como entrada ya no será necesario indicar la _interfaz lógica_ a la que se hace referencia ya que la herramienta se encargará de probar todos los _perfiles_ en paralelo y elegirá aquella que cumpla en primera instancia con los casos de prueba. De modo tal que ya podremos dar de alta nuestra _interfaz física_ con solo hacer `ifup wlan0`.
