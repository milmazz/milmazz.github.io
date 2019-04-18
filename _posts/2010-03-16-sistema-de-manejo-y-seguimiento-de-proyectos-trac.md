---
title: 'Sistema de manejo y seguimiento de proyectos: Trac'
author: milmazz
date: 2010-03-16 01:24:43
categories:
  - Programación
  - Software
tags:
  - Ingeniería del Software
  - Programación
  - Software
  - trac
slug: sistema-de-manejo-y-seguimiento-de-proyectos-trac
redirect_from: /archivos/2010/03/16/sistema-de-manejo-y-seguimiento-de-proyectos-trac/
---

[Trac](http://trac.edgewall.org) es un sistema multiplataforma desarrollado y mantenido por [Edgewall Software](http://edgewall.org), el cual está orientado al seguimiento y manejo de proyectos de desarrollo de _software_ haciendo uso de un enfoque minimalista basado en la _Web_, su misión es ayudar a los desarrolladores a escribir software de excelente calidad mientras busca no interferir con el proceso y políticas de desarrollo. Incluye un sistema _wiki_ que es adecuado para el manejo de la base de conocimiento del proyecto, fácil integración con sistemas de control de versiones ((Por defecto Trac se integra con _subversion_)). Además incluye una interfaz para el seguimiento de tareas, mejoras y reporte de errores por medio de un completo y totalmente personalizable sistema de _tickets_, todo esto con el fin de ofrecer una interfaz integrada y consistente para acceder a toda información referente al proyecto de desarrollo de software. Además, todas estas capacidades son extensibles por medio de _plugins_ o complementos desarrollados específicamente para Trac.

## Breve historia de Trac

El origen de Trac no es una idea original, algunos de sus objetivos se basan en los diversos sistemas de manejo y seguimiento de errores que existen en la actualidad, particularmente del sistema [CVSTrac](http://cvstrac.org/) y sus autores.

Trac comenzó como la [reimplementación](http://trac.edgewall.org/wiki/TracHistory) del sistema CVSTrac en el lenguaje de programación [Python](http://www.python.org) y como ejercicio de entretenimiento, además de utilizar la base de datos embebida [SQLite](http://www.sqlite.org) ((Hoy día también se le da soporte a [PostgreSQL](http://www.postgresql.org), mayor detalle en [Database Backend](http://trac.edgewall.org/wiki/DatabaseBackend))). En un corto lapso de tiempo, el alcance de estos esfuerzos iniciales crecieron en gran medida, se establecieron metas y en el presente Trac presenta un curso de desarrollo propio.

Los desarrolladores de _Edgewall Software_ esperan que Trac sea una plataforma viable para explorar y expandir el cómo y qué puede hacerse con sistemas de manejo de proyectos de desarrollo de software basados en sistemas wiki.

## Características de Trac

A continuación se presenta una descripción breve de las distintas características de Trac.

### Herramienta de código abierto para el manejo de proyectos

Trac es una herramienta ligera para el manejo de proyectos basada en la _Web_, desarrollada en el lenguaje de programación Python. Está enfocada en el manejo de proyectos de desarrollo de software, aunque es lo suficientemente flexible para usarla en muchos tipos de proyectos. Al ser una herramienta de [código abierto](http://trac.edgewall.org/wiki/TracLicense), si Trac no llena completamente sus necesidades, puede aplicar los cambios necesarios usted mismo, escribir complementos o _plugins_, o contratar a alguien calificado que lo haga por usted.

## Sistema de tickets

![Tickets activos para el hito 0.12 de Trac, ordenados por última fecha de modificación](/images/2010-03-16-sistema-de-manejo-y-seguimiento-de-proyectos-trac/tickets-300x193.png)

El sistema de _tickets_ le permite hacer seguimiento del progreso en la resolución de problemas de programación particulares, requerimientos de nuevas características, problemas e ideas, cada una de ellas en su propio _ticket_, los cuales son enumerados de manera ascendente. Puede resolver o reconciliar aquellos _tickets_ que buscan un mismo objetivo o donde más de una persona reporta el mismo requerimiento. Permite hacer búsquedas o filtrar _tickets_ por severidad, componente del proyecto, versión, responsable de atender el _ticket_, entre otros.

Para mejorar el seguimiento de los _tickets_ Trac ofrece la posibilidad de activar notificaciones vía correo electrónico, de este modo se mantiene informado a los desarrolladores de los avances en la resolución de las actividades planificadas.

## Vista de progreso

Existen varias maneras convenientes de estar al día con los acontecimientos y cambios que ocurren dentro de un proyecto. Puede establecer hitos y ver un mapa del progreso (así como los logros históricos) de manera resumida. Además, puede visualizar desde una interfaz centralizada los cambios ocurridos cronológicamente en el wiki, hitos, tickets y repositorios de código fuente, comenzando con los eventos más recientes, toda esta información es accesible vía _Web_ o de manera alternativa Trac le ofrece la posibilidad de exportar esta información a otros formatos como el RSS, permitiendo que los usuarios puedan observar esos cambios fuera de la interfaz centralizada de Trac, así como la notificación por correo electrónico.

![vista de progreso del proyecto](/images/2010-03-16-sistema-de-manejo-y-seguimiento-de-proyectos-trac/milestone-300x221.png "Vista del avance del proyecto para un hito particular")

## Vista del repositorio en línea

Una de las características de mayor uso en Trac es el navegador o visor del repositorio en línea, se le ofrece una interfaz bastante amigable para el sistema de control de versiones que esté usando ((Por defecto Trac se integra con _subversion_, la integración con otros sistemas es posible gracias a _plugins_ o complementos.)). Este visualizador en línea le ofrece una manera clara y elegante de observar el código fuente resaltado, así como también la comparación de ficheros, apreciando fácilmente las diferencias entre ellos.

![Visor de código fuente en Trac](/images/2010-03-16-sistema-de-manejo-y-seguimiento-de-proyectos-trac/code-275x300.png "Cambios entre revisiones en trunk/setup.py")

## Manejo de usuarios

Trac ofrece un sistema de permisología para controlar cuales usuarios pueden acceder o no a determinadas secciones del sistema de manejo y seguimiento del proyecto, esto se logra a través de la interfaz administrativa. Además, esta interfaz es posible integrarla con la definición de permisos de lectura y escritura de los usuarios en el sistema de control de versiones, de ese modo se logra una
administración centralizada de usuarios.

## Wiki

El sistema wiki es ideal para mantener la base de conocimientos del proyecto, la cual puede ser usada por los desarrolladores o como medio para ofrecerles recursos a los usuarios. Tal como funcionan otros sistemas wiki, puede permitirse la edición compartida. La sintaxis del sistema wiki es bastante sencilla, si esto no es suficiente, es posible integrar en Trac un editor WYSIWYG (lo que se ve es lo que se obtiene, por sus siglas en inglés) que facilita la edición de los documentos.

## Características adicionales

Al ser Trac un sistema modular puede ampliarse su funcionalidad por medio de complementos o _plugins_, desde sistemas anti-spam hasta diagramas de Gantt o sistemas de seguimiento de tiempo.
