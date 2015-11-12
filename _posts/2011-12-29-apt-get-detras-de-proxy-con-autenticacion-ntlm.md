---
redirect_from: "/archivos/2011/12/29/apt-get-detras-de-proxy-con-autenticacion-ntlm/"
author: milmazz
comments: true
date: 2011-12-29 17:16:59
layout: post
slug: apt-get-detras-de-proxy-con-autenticacion-ntlm
title: apt-get detrás de proxy con autenticación NTLM
wordpress_id: 425
categories:
- debian
tags:
- apt-get
- cntlm
- debian
- linux
- proxy
---

Por motivos que no vienen al caso discutir en este artículo tuve que instalar Debian GNU/Linux detrás de un _proxy_ que aún utiliza NTLM como medio de autenticación, aunque NTLM ya no es recomendado por Microsoft desde hace años en pro de usar [Kerberos](http://web.mit.edu/kerberos/).

Una vez instalada la distribución quería utilizar `apt-get` para actualizarla e instalar nuevos paquetes, el resultado fue que `apt-get` no funciona de manera transparente detrás de un _proxy_ con autenticación NTLM. La solución fue colocar un _proxy interno_ que esté atento a peticiones en un puerto particular en el _host_, el _proxy interno_ se encargará de proveer de manera correcta las credenciales al _proxy externo_.

La solución descrita previamente resulta sencilla al utilizar `cntlm`. En principio será necesario instalarlo vía `dpkg`, posteriormente deberá editar los campos apropiados en el fichero `/etc/cntlm.conf`

  * Username
  * Domain
  * Password
  * Proxy

Seguidamente reinicie el servicio:
    
    # /etc/init.d/cntlm restart

Ahora solo resta configurar `apt-get` para que utilice nuestro _proxy interno_, para ello edite el fichero _/etc/apt.conf.d/02proxy_

    Acquire::http::Proxy "http://127.0.0.1:3128";

**NOTA:** Se asume que el puerto de escucha de `cntlm` es el _3128_.

Ahora puede hacer uso correcto de `apt-get`:

    # apt-get update
    # apt-get upgrade
    ...

**NOTA FINAL:** Es evidente que cualquier comando o herramienta que necesite autenticarse contra el _proxy externo_ deberá configurarlo para que utilice el _proxy interno_, lo explicado en este artículo no solo aplica para el comando `apt-get`.
