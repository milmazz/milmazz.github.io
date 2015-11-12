---
redirect_from: "/archivos/2006/01/12/vulnerabilidad-en-apache/"
author: milmazz
comments: true
date: 2006-01-12 14:22:06
layout: post
slug: vulnerabilidad-en-apache
title: Vulnerabilidad en Apache
wordpress_id: 128
categories:
- Seguridad
- Ubuntu
tags:
- apache
- linux
- Seguridad
- Ubuntu
---

Según un [anuncio](http://lists.ubuntu.com/archives/ubuntu-security-announce/2006-January/000276.html) hecho el día de hoy por Adam Conrad a la [lista de seguridad de ubuntu](http://lists.ubuntu.com/mailman/listinfo/ubuntu-security-announce) existe una vulnerabilidad que podría permitirle a una página web maligna (o un correo electrónico maligno en `HTML`) utilizar técnicas de [Cross Site Scripting](http://es.wikipedia.org/wiki/Cross_Site_Scripting) en [Apache](http://apache.org/). Esta vulnerabilidad afecta a las versiones: Ubuntu 4.10 (Warty Warthog), Ubuntu 5.04 (Hoary Hedgehog) y Ubuntu 5.10 (Breezy Badger).

De manera adicional, Hartmut Keil descubre una vulnerabilidad en el módulo SSL (`mod_ssl`), que permitiría realizar una [denegación de servicio](http://es.wikipedia.org/wiki/DoS) (DoS), lo cual pone en riesgo la integridad del servidor. Esta última vulnerabilidad solo afecta a apache2, siempre y cuando esté usando la implementación "worker" (`apache2-mpm-worker`).

Los paquetes afectados son los siguientes:

  * `apache-common`
  * `apache2-common`
  * `apache2-mpm-worker`

Los problemas mencionados previamente pueden ser corregidos al actualizar los paquetes mencionados.
