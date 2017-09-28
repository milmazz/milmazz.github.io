---
redirect_from: "/archivos/2005/05/03/es-necesario-activar-la-cuenta-root-en-ubuntu/"
author: milmazz
comments: true
date: 2005-05-03 11:50:01
layout: post
slug: es-necesario-activar-la-cuenta-root-en-ubuntu
title: ¿Es necesario activar la cuenta root en Ubuntu?
categories:
- Ubuntu
tags:
- Ubuntu
---

Los desarrolladores de [Ubuntu Linux](http://ubuntulinux.org/) en un principio querían que el proceso de instalación fuese lo más fácil posible, el hecho de desactivar de manera predeterminada la cuenta de usuario `root` permitía obviar algunos pasos en el proceso de instalación. Esto para muchos es un inconveniente, pues Ubuntu Linux "difiere" en cuanto al modelo de seguridad que se maneja comúnmente en sistemas GNU/Linux, el modelo que plantea Ubuntu Linux es simplemente no recomendar hacer uso extensivo del usuario `root` (por eso ha desactivado la cuenta). Debido a que esta distribución está orientada hacia usuarios que quizás no han tenido un contacto extenso con sistemas GNU/Linux, el modelo propuesto me parece bastante lógico.

Por supuesto, este modelo presenta ventajas y desventajas, desde mi punto de vista son más ventajas que desventajas, ¿por qué?, a continuación detallo mis argumentos.

  * Ubuntu Linux está orientada hacia usuarios finales que no han tenido tanto contacto con el mundo GNU/Linux, seguramente estos nuevos usuarios no están adaptados al modelo de seguridad que se maneja en estos sistemas, por lo que seguramente se generen más olvidos a la hora de recordar la contraseña que se utiliza para fines administrativos, puesto que esta cuenta es pocas veces usadas por ellos. Con el uso de sudo (Super User DO) esto no pasa, puesto que se maneja la misma contraseña del usuario principal (o aquellos que estén autorizados) para fines administrativos.
  * Seguramente el hecho de introducir la contraseña para realizar cambios administrativos te detenga a pensar en lo que estas haciendo realmente, lo cual puede reducir la tasa de errores en la administración del sistema.
  * Puedes ver un registro de las actividades que se realizan con el comando `sudo` en `/var/log/auth.log`, lo cual puede ayudar a administrar tu sistema eficientemente.

He escuchado argumentos de personas que dicen que el usar `sudo` es tedioso, puesto que debes introducir la constraseña a cada instante, eso no es cierto, la primera vez que introduzcas la contraseña, ésta se almacenará por quince (15) minutos, después de transcurrido ese tiempo y si necesitas hacer alguna actividad administrativa se te volverá a solicitar.

Quizás la mayor desventaja de este modelo es que el mantener una contraseña "diferente" para el superusuario existe mayor protección en el caso en que las contraseñas de los usuarios con derehos administrativos se vean comprometidas. Pero esto puede ser evitado al ponerle mayor cuidado a las cuentas de usuarios con derechos administrativos, la debilidad es allí y no en el modelo en cuestión.

Lo cierto es que el uso de `sudo` puede considerarse para ejecutar pocos comandos administrativos, mientras que su generalmente es utilizado para ejecutar múltiples tareas administrativas, el problema es que su puede dejar "abierta" indefinidamente una _shell_ con derechos de superusuario, esto último es un gran **inconveniente** para la seguridad del sistema. En cambio sudo limita estas cosas, como se menciono anteriormente, al menos tienes quince (15) minutos de derechos de superusuario.
Si a pesar de lo que he mencionado hasta ahora, ud. considera conveniente activar la cuenta de superusuario en Ubuntu Linux, aca está la serie de pasos que deberá seguir:

  1. sudo passwd root
  2. Cambiar la configuración de sudo, para evitar que el usuario principal haga uso de él, este paso puede ser opcional, aunque es recomendable hacerlo si realmente se desea hacer la "separación" a la cual estamos acostumbrados en los sistemas GNU/Linux.
  3. Cambiar las entradas del menú que hacen uso de `gksudo` (comúnmente aquellas aplicaciones con fines administrativos) por `gksu`, para que realmente pidan la contraseña de root y no la del usuario principal.

Como conclusión, desde mi punto de vista considero innecesario tomarse tantas molestias para activar al usuario root en Ubuntu Linux cuando el mecanismo propuesto (sudo) funciona perfectamente.
