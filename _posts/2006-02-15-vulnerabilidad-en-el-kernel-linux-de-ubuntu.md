---
redirect_from: "/archivos/2006/02/15/vulnerabilidad-en-el-kernel-linux-de-ubuntu/"
author: milmazz
comments: true
date: 2006-02-15 09:45:46
layout: post
slug: vulnerabilidad-en-el-kernel-linux-de-ubuntu
title: Vulnerabilidad en el kernel Linux de ubuntu
wordpress_id: 138
categories:
- Seguridad
- Ubuntu
tags:
- kernel
- linux
- Seguridad
- Ubuntu
---

Este problema de seguridad únicamente afecta a la distribución Ubuntu 5.10, [Breezy Badger](http://www.ubuntu.com/news/release510).

Los paquetes afectados son los siguientes:

  * `linux-image-2.6.12-10-386`
  * `linux-image-2.6.12-10-686`
  * `linux-image-2.6.12-10-686-smp`
  * `linux-image-2.6.12-10-amd64-generic`
  * `linux-image-2.6.12-10-amd64-k8`
  * `linux-image-2.6.12-10-amd64-k8-smp`
  * `linux-image-2.6.12-10-amd64-xeon`
  * `linux-image-2.6.12-10-iseries-smp`
  * `linux-image-2.6.12-10-itanium`
  * `linux-image-2.6.12-10-itanium-smp`
  * `linux-image-2.6.12-10-k7`
  * `linux-image-2.6.12-10-k7-smp`
  * `linux-image-2.6.12-10-mckinley`
  * `linux-image-2.6.12-10-mckinley-smp`
  * `linux-image-2.6.12-10-powerpc`
  * `linux-image-2.6.12-10-powerpc-smp`
  * `linux-image-2.6.12-10-powerpc64-smp`
  * `linux-patch-ubuntu-2.6.12`


El problema puede ser solucionado al actualizar los paquetes afectados a la versión **2.6.12-10.28**. Posterior al proceso de actualización debe reiniciar el sistema para que los cambios logren surtir efecto.

Puede encontrar mayor detalle acerca de esta información en el anuncio [Linux kernel vulnerability](https://lists.ubuntu.com/archives/ubuntu-security-announce/2006-February/000285.html) hecho por _Martin Pitt_ a la lista de correos de [avisos de seguridad en Ubuntu](https://lists.ubuntu.com/mailman/listinfo/ubuntu-security-announce).
