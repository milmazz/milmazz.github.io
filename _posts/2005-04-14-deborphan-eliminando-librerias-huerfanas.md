---
title: deborphan, eliminando librerias huérfanas
author: milmazz
date: 2005-04-14 16:30:39
categories:
  - GNU/Linux
  - Ubuntu
tags:
  - GNU/Linux
  - Ubuntu
slug: deborphan-eliminando-librerias-huerfanas
redirect_from: /archivos/2005/04/14/deborphan-eliminando-librerias-huerfanas/
---

**deborphan** simplemente se encarga de buscar librerias huérfanas. Se mostrará un listado de paquetes que posiblemente no fueron desinstalados en su momento y que actualmente **no** son requeridas en el sistema por ningún otro paquete o aplicación. Esta aplicación es realmente útil para mantener "limpio" el sistema en caso de ser necesario.

Para instalar **deborphan** en nuestro sistema simplemente debe proceder como sigue:

    sudo apt-get install deborphan

Para remover todos los paquetes huérfanos encontrados simplemente debemos proceder como sigue:

    sudo deborphan | sudo xargs apt-get remove -y

Esta aplicación me ha sido realmente útil, sobretodo por la reciente actualización que he realizado en la versión mi distribución [Ubuntu Linux](http://www.ubuntulinux.org/), la versión anterior era _Warty Warthog_ (v.4.10), la nueva es _Hoary Hedgehog_ (v.5.04). En dicha actualización ocurrieron muchos "rompimientos" de dependencias, muchas librerias quedaron huérfanas, con esta aplicación he depurado el sistema.
