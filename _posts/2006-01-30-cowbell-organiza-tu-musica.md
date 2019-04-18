---
title: 'Cowbell: Organiza tu música'
author: milmazz
date: 2006-01-30 08:00:40
categories:
  - Software
  - Ubuntu
tags:
  - cowbell
  - linux
  - music
  - Software
  - Ubuntu
slug: cowbell-organiza-tu-musica
redirect_from: /archivos/2006/01/30/cowbell-organiza-tu-musica/
---

[Cowbell](http://more-cowbell.org/), es una aplicación que te permite organizar tus compilaciones musicales de una manera fácil y divertida, ya no tienes que aburrirte por horas al intentar organizar tus colección musical manualmente.

Una de las cosas que me han agradado de este programa es que aparte de poder editar las etiquetas manualmentede en una interfaz bastante agradable y sencilla, también puedes obtener toda la información necesaria a través de _Amazon Web Services_, lo anterior incluye: Número, Título, Año, Estilo, Portada y demás información relacionada con las canciones. Al utilizar este servicio cuentas con una amplia bases de datos, lo anterior en realidad permite ahorrar mucho tiempo.

Dentro de las preferencias de este programa nos encontraremos con opciones que nos permitirán renombrar ficheros de acuerdo a un patrón, el cual lo podemos generar al combinar cualquiera de las siguientes palabras claves.

  * _Artist_
  * _Album_
  * _Title_
  * _Track_
  * _Genre_
  * _Year_

Las palabras claves anteriores se explican por sí solas. Simplemente escoge el patrón que más se ajuste a tus necesidades. Entre otras de las características de este programa, cabe mencionar la posibilidad de generar un fichero de _lista de reproducción_ del álbum.

¿Tienes una larga colección de música cuyas etiquetas debes arreglar?, no te preocupes, Cowbell también puedes usar desde la línea de comandos, la manera de _invocar_ el comando es la siguiente:

    $ cowbell --batch /ruta/a/tu/musica

Donde evidentemente debes modificar el directorio `/ruta/a/tu/musica` de acuerdo a tus necesidades.

Para instalar esta aplicación en ubuntu debes tener activo el repositorio `universe` en tu fichero `/etc/apt/sources.list`. Una vez actualizada la lista de repositorios, puedes instalar Cowbell de la siguiente manera:

    $ sudo aptitude cowbell
