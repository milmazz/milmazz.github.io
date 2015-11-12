---
redirect_from: "/archivos/2006/03/12/vulnerabilidad-grave-corregida-en-ubuntu/"
author: milmazz
comments: true
date: 2006-03-12 21:33:19
layout: post
slug: vulnerabilidad-grave-corregida-en-ubuntu
title: Vulnerabilidad grave corregida en ubuntu
wordpress_id: 150
categories:
- Seguridad
- Ubuntu
tags:
- bug
- root
- security
- Seguridad
- Ubuntu
---

La vulnerabilidad en ubuntu donde [cualquiera con acceso al shell podía ver la contraseña del instalador del sistema](https://launchpad.net/distros/ubuntu/+bug/34606) ha sido reparada.

Los paquetes afectados son `base-config` y `passwd` en [Ubuntu 5.10](http://www.ubuntu.com/news/release510) (Breezy Badger), el problema puede ser corregido al actualizar los paquetes afectados a las versiones `2.67ubuntu20` (`base-config`) y `1:4.0.3-37ubuntu8` (`passwd`). En general, una actualización del sistema es suficiente para que los cambios surtan efecto.

La vulnerabilidad en donde era posible ver la constraseña del instalador de ubuntu y así tener acceso a privilegios de _superusuario_ (por supuesto, si la contraseña no había sido cambiada desde la instalación) fue descubierta por Karl Øie, el instalador de Ubuntu 5.10 falla en la limpieza de contraseñas en los ficheros de registros de instalación. Dichos ficheros pueden ser leidos por cualquiera, de este modo cualquier usuario local pudiese ver las contraseñas del primer usuario, el cual tiene **privilegios por completo** en una instalación por defecto.

El paquete actualizado eliminará las contraseñas almacenadas en texto plano en los ficheros de registro de instalación y adicionalmente hará que los registros sean leídos únicamente por el _superusuario_.

Esta vulnerabilidad no afecta a los instaladores de las versiones `4.10`, `5.04`, o la que está por llegar, `6.04`. Sin embargo, considere necesario aplicar el parche si usted ha actualizado su sistema desde Ubuntu `5.10` a la actual versión en desarrollo `6.04` (Dapper Drake), en este caso verifique que ha actualizado el paquete `passwd` a la versión `1:4.0.13-7ubuntu2`.
