---
redirect_from: "/archivos/2005/11/21/ctrl-alt-supr-en-ubuntu/"
author: milmazz
comments: true
date: 2005-11-21 22:56:44
layout: post
slug: ctrl-alt-supr-en-ubuntu
title: Ctrl + Alt + Supr en Ubuntu
wordpress_id: 103
categories:
- Ubuntu
tags:
- Ubuntu
---

Si usted alguna vez llegó a preguntarse como lograr abrir el **Monitor del Sistema** al presionar las teclas Ctrl + Alt + Supr, comportamiento similar bajo sistemas Windows, en el [Blog de Bairsairk](http://bairsairk.org/) responden dicha inquietud.

Simplemente debe utilizar los siguientes comandos bajo el _Terminal_.

    $ gconftool-2 -t str --set /apps/metacity/global_keybindings/run_command_9 "<Control><alt>Delete"
    $ gconftool-2 -t str --set /apps/metacity/keybinding_commands/command_9 "gnome-system-monitor"

Simplemente copie y pegue :)
