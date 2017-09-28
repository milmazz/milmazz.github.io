---
redirect_from: "/archivos/2005/10/07/evolution-y-gmail/"
author: milmazz
comments: true
date: 2005-10-07 05:54:32
layout: post
slug: evolution-y-gmail
title: Evolution y Gmail
categories:
- GNU/Linux
- Software
tags:
- GNU/Linux
- Software
---

Si desea configurar el cliente de correo
[Evolution](http://gnome.org/projects/evolution/)  (también brinda una agenda de
contactos, calendario, entre otras funcionalidades) para manejar su cuenta de
[Gmail](http://www.gmail.com/), estos son los pasos.

## Habilitando el acceso POP en su cuenta de [Gmail](http://www.gmail.com/)

1. Identificarse en [Gmail](http://www.gmail.com/).
1. Un vez dentro del sistema, ir a la opción de Configuración
1. Seguidamente proceda a seleccionar Reenvío y correo POP del menú.
1. Dentro de la sección Descargar correo POP encontramos tres derivaciones:
  * La primera se refiere al **Estado**, en ella debemos habilitar cualquiera de
    las dos opciones que se muestran al principio, la primera permite Habilitar
    POP para todos los mensajes (incluso si ya se han descargado), la segunda
    opción permite Habilitar POP para los mensajes que se reciban a partir de
    ahora.
  * La segunda derivación se refiere a qué debe hacer
    [Gmail](http://www.gmail.com/) cuando se accede a los mensajes a través de
    POP, eliga la respuesta de su conveniencia, yo por lo menos tengo conservar
    una copia de Gmail en la bandeja de entrada.
  * La tercera derivacion se refiere a como lograr configurar el cliente de
    correo electrónico, en nuestro caso, será
    [Evolution](http://gnome.org/projects/evolution/).
1. Guardar cambios.

Ahora vamos a configurar nuestra cuenta [Gmail](http://www.gmail.com/) desde
[Evolution](http://gnome.org/projects/evolution/). En primer lugar veamos como
configurar la recepción de correos.

## Recibiendo Mensajes

* Tipo de servidor: POP
* Servidor: `pop.gmail.com`
* Usuario: _nombredeusuario@gmail.com_, evidentemente debe cambiar la cadena
  _nombredeusuario_ por su _login_ verdadero, no olvide colocar seguido del
  nombre de usuario la cadena _@gmail.com_.
* Usar onexión segura: Siempre
* Tipo de autenticación: Password

Ahora veamos como configurar el envio de correos desde
[Evolution](http://gnome.org/projects/evolution/).

## Enviando correos

* Tipo de servidor: SMTP
* Servidor: Puede usar las siguientes: `smtp.gmail.com`, `smtp.gmail.com:587` ó
  `smtp.gmail.com:465`. Debe marcar la casilla de verificación El servidor
  requiere autenticación
* Usar conexión segura: Cuando sea posible
* Tipo (dentro de la sección de autenticación): Login
* Usuario (dentro de la sección de autenticación): _nombredeusuario@gmail.com_,
  recuerde sustituir la cadena _nombredeusuario_ por el parámetro
  correspondiente, no olvide colocar después del nombre de usuario la cadena
  _@gmail.com_, es importante.

Finalmente revise las opciones que le brinda
[Evolution](http://gnome.org/projects/evolution/) y comience una vida llena de
placeres.
