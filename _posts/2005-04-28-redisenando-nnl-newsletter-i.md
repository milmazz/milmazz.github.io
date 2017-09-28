---
author: milmazz
comments: false
date: 2005-04-28 06:07:55
layout: post
slug: redisenando-nnl-newsletter-i
title: Rediseñando NNL Newsletter I
tags:
- (X)HTML
---

Hace ya bastante tiempo que llevo siguiendo esta interesante lista de correo, orientada sobretodo al mundo IT, tambien se publican noticias relacionadas al mundo GNU/Linux, lo cual capto mi atencion de inmediato.




Pero este articulo no tratara sobre el excelente contenido que presenta regularmente Carlos Tori (el hombre detras de [NNL Newsletter](http://www.nnlnews.com/)), este articulo tratara mas bien una propuesta de rediseño en los boletines que se presentan.




El rediseño que le plantee a Carlos Tori captó su atencion, aun no  se ha implantado en el sitio oficial debido a mi "retraso" (algo en lo que mis estudios en la Universidad ha tenido mucho que ver) en la depuracion de los actuales 25 boletines, queda aun mucho trabajo por hacer, solo quiero mostrarle una anticipacion a mis dos fieles lectores. Espero les guste.


<!-- more -->


### Comenzamos




Si observamos los boletines que presenta [NNL Newsletter](http://www.nnlnews.com/) podemos darnos cuenta que aun mantienen el formato usual de mensajes de correo electronico, todo esto a pesar del avance que ha presentado la lista, ahora se puede catalogar como un sitio web en donde se podra leer noticias interesantes, dichas noticias aparecen cada cierto tiempo, agrupadas en un mismo documento, al final de cada documento se anexa el soporte que le ha brindado Carlos Tori a sus fervientes lectores.




Desde mi perspectiva debe darse un "lavado de cara" al Newsletter. Comencemos por la semántica de los documentos. Tomare como muestra el [NNL Newsletter » 1](http://www.nnlnews.com/nnl/1/).




### Eliminando lo innecesario




En primer lugar elimine los estilos incrustados en el mismo documento, la ventaja que  nos otorga el uso de hojas de estilos en cascada o CSS es separar la estructura que describe el contenido del documento de su presentacion. Por lo cual, controlaremos la presentacion de los 25 boletines (y posteriores) a través de un solo documento CSS. A continuacion se muestra el bloque eliminado:



    
    <code><style type="text/css">
    <!--
    .style1 {
      font-family: "Courier New", Courier, mono;
      font-size: 12px;
    }
    .style2 {color: #000000}
    .style11 {font-size: 1px}
    .style12 {color: #FFFFFF}
    -->
    </style></code>




Posteriormente procedi a eliminar el uso excesivo de las etiquetas `<br>`, de igual manera se eliminaron el uso de las clases: `style1`, `style2`, `style11` y `style12`. Esto es realmente sencillo hacerlo, simplemente buscamos en nuestro editor favorito (en mi caso: [Bluefish](http://bluefish.openoffice.nl/)) la opcion de Reemplazo (Edicion -> Reemplazar), introducimos los parametros de busqueda y reemplazo y el editor hara el trabajo por nosotros.




Siguiendo con el proceso de eliminacion nos encontramos con el **logo** de NNL Newsletter, el cual tiene ciertos inconvenientes:



    
    <code><p><img src="../logo.png" width="134" height="130"></p></code>




El primer inconveniente es el semantico, en realidad un Agente de Usuario no sabra definir la importancia de esta parte del codigo, simplemente lo tratara como un parrafo mas, adicionalmente, se comete el error de emplear una imagen que el Agente de Usuario (P.ej. Un buscador) no sabria interpretar, ya que no se presenta siquiera un texto alternativo, el uso del atributo `alt` en las imagenes es vital. Ahora bien, lo que en realidad le daria la importancia que se merece esta parte del codigo seria usar un encabezado que le corresponda.



    
    <code><h1><span></span>NNL Newsletter</h1></code>




¡Ahora si!, hemos conseguido darle un alto nivel de importancia al nombre del newsletter, ahora bien, muchos se preguntaran, ¿Por que de manera tan drastica ha eliminado la imagen?, ¿Acaso para lograr documentos realmente semanticos debemos deshacernos de las imagenes?, desde mi perspectiva el uso de imagenes con fines superfluos o unicamente con fines presentacionales deben dejarse del lado de las hojas de estilo en cascada, simplemente se hara (posteriormente) un reemplazo del encabezado por la imagen via CSS (por eso de la etiqueta `span` vacia en el encabezado `h1`), este reemplazo via CSS no disminuira el valor semantico de nuestro documento.





### Haciendo algunos cambios necesarios




Carlos Tori, mantiene un formato en su newsletter, normalmente, da una breve introduccion, posteriormente presenta una lista ordenada de los topicos a los cuales hara mencion en el boletin. Seguidamente procede a desglosar cada uno de esos topicos, entre otras cosas. Podemos cambiar ciertas cosas en el boletin.




Primero, la lista ordenada que presenta el newsletter comunmente, la unica salvedad son los diferentes topicos a los cuales puede hacer mencion.



    
    <code><p>1- Nota: CGI inseguro<br>
    2- Pequeños grandes consejos al admin: .bash_history<br>
    3- Mini análisis de xploits privados: RPC Sunos - 0day -<br>
    4- Feedback: Los lectores preguntaron...<br>
    5- N de la R. ( Notas de la redacción. ) / Next</p></code>




De manera visual puede que parezca una lista, pero debemos darle a las cosas el significado que tienen en realidad, es decir, le agregaremos un valor semantico, de esta manera sera comprensible para cualquier Agente de Usuario la estructura del contenido mostrado.



    
    <code><ol>
    <li><a href="#nnl_1_1" title="Vea el 
    tópico">CGI inseguro</a></li> 
    <li><a href="#nnl_1_2" title="Vea el
    tópico">Peque&ntilde;os grandes consejos al admin:
    .bash_history</a></li> 
    <li><a href="#nnl_1_3" title="Vea el
    tópico">Mini an&aacute;lisis de xploits privados:
    RPC Sunos -0day-</a></li>
    <li><a href="#nnl_1_4" title="Vea el
    tópico">Feedback: Los lectores preguntaron...</a></li> 
    <li><a href="#nnl_1_5" title="Vea el
    tópico"><abbr title="Notas de la Redacción"
    xml:lang="es">N de la R.</abbr> (Notas
    de la Redacci&oacute;n) / Next</a></li>
    </ol></code>




En la lista ordenada mostrada arriba debemos hacer notar el uso de las [entidades de caracteres](http://www.sidar.org/recur/desdi/traduc/es/html401-es/charset.html#h-5.3.2). Adicionalmente hemos mejorado la [semantica de la abreviacion](http://www.bitacoradewebmaster.com/index.php?p=154) que hace el autor acerca de las **Notas de la Redaccion**. Por ultimo, pero no menos importante, es el uso de vinculos que llevan como valor (precediendole el caracter almohadilla '#') en el atributo `href` el valor de los atributos `id` de otros elementos, esto beneficiara a aquel lector que desea dedicarse a una parte especifica del newsletter, no debera desplazarse por todo el documento, solo debera dar click en el enlace y este le llevara a la parte requerida, es un gran beneficio para el lector, ya que en algunas ocasiones el newsletter presenta un contenido realmente extenso.




En este punto eliminaremos ciertos separadores que son utilizados para seccionar los topicos y le daremos el significado correcto a los titulos de cada topico. Debemos considerar que es una lista de puntos a los cuales se le da una definicion, por lo cual, me parece bastante adecuado utilizar una [lista de definiciones](http://www.sidar.org/recur/desdi/traduc/es/html401-es/struct/lists.html#h-10.3) en este caso.




Version de Carlos Tori.



    
    <code><p>***************************************************
    ((2)) Pequeños grandes consejos al admin: ".bash_history"</p>
    <p>Amigo administrador: Sabias que una forma rápida de
    conseguir privilegios de root es leyendo los .bash_history
    de los usuarios de tu server ? Sí, en efecto es asi.
    Muchos admins y users pastean sus dificilisimos passwords
    durante algún lapsus mental tipo "su ##@|2ks89u" o
    "su root##@|2ks89u" o bien pastean el pass "##@|2ks89u" y
    dan enter... y lo dejan allí, a la espera de que alguien
    lo lea. Controlen a los usuarios que disponen de dicho
    pass, y mas aún sus ".bash_history". Amén.</p></code>




Ahora, presentamos una mejora de la version anterior:



    
    <code><dl>
    ...
    <dt id="nnl_1_2">Peque&ntilde;os
    grandes consejos al admin: <code>.bash_history</code></dt>
    <dd><p>Amigo administrador: &iquest;Sab&iacute;as 
    que una forma r&aacute;pida de
    conseguir privilegios de <code>root</code> es leyendo los <code>.bash_history</code>
    de los usuarios de tu server? S&iacute;, en efecto es asi.</p>
    <p>Muchos admins y users pastean sus dificilisimos passwords 
    durante alg&uacute;n lapsus mental tipo
    <code>su ##@|2ks89u</code> o
    <code>su root##@|2ks89u</code> o bien pastean el pass <code>##@|2ks89u</code> y
    dan enter... y lo dejan all&iacute;, a la espera de que alguien
    lo lea. Controlen a los usuarios que disponen de dicho
    pass, y mas a&uacute;n sus <code>.bash_history</code>. Am&eacute;n.</p></dd>
    ...
    </dl></code>




En principio, a cada elemento `dt` le colocamos el atributo `id`, con el fin de poder enlazar a dicho elemento desde otra ubicacion, en nuestro caso, lo hacemos desde la lista ordenada presentada mas arriba, esto facilitara el desplazamiento por el documento, sobretodo si un lector desea dedicarse en especial a este topico.




Segun la especificacion, las listas de definiciones consisten en dos partes, la primera de ellas sera el termino a definir, la segunda parte es la definicion en si. El termino a definir (dado por `dt`) **puede contener** _elementos en linea_, mas **no** en _elementos en bloque_. En cambio, la definicion del termino contiene _elementos en bloque_.



    
    <code><p>***************************************************</p>
    <p>((4)) Feedback: Los lectores preguntaron...
    ---------------------------------------------------
    From: MP emonicap@*
    To: nnl@hushmail.com
    Subject: Consulta disquetera
    Gracias por permitir consultas :)
    Saben si hay alguna manera por software de inhabilitar
    la escritura en la disquetera?
    Desde ya gracias
    Mónica</p>
    <p>R: Hablando de una simple plataforma Windows como opciones
    tenes varias... o bien podes deshabilitarla desde
    propiedades en "MI PC", sino hablando en un plano command-
    line tenes Floplock.exe ( Lock Floppy Disk Drives )
    
    disponible en el resource kit de NT o por ultimo
    > http://www.protect-me.com/dl/ - software -</p></code>




Haremos algunos cambios en la estructura anterior...



    
    <code><dl>
    ...
      <dt id="nnl_1_4">Feedback: Los lectores preguntaron...</dt>
        <dd id="feedback">
          <dl>
            <dt class="ask">From: MP emonicap@*</dt>
            <dd class="message">
              <p class="to"><strong>To:</strong> nnl@hushmail.com</p>
              <p class="subject"><strong>Subject:</strong> Consulta disquetera</p>
              <p>Gracias por permitir consultas :)</p>
              <p>&iquest;Saben si hay alguna manera por software de inhabilitar
            la escritura en la disquetera?</p>
              <p>Desde ya gracias,
              Monica</p>
          </dd>
          <dt class="ans"><strong>From:</strong> nnl@hushmail.com</dt>
          <dd class="nnl">
          <p><strong><abbr title="Respuesta"
          xml:lang="es">R:&gt;</abbr></strong> 
          Hablando de una simple plataforma Windows como opciones tenes varias... 
          o bien podes deshabilitarla desde propiedades en &quot;MI PC&quot;, 
          sino hablando en un plano command-line tenes Floplock.exe 
          (<span xml:lang="en">Lock Floppy Disk Drives</span>) 
          disponible en el resource kit de NT o por &uacute;ltimo 
          <a href="http://www.protect-me.com/dl/"
          hreflang="en">http://www.protect-me.com/dl/</a>
          -software-</p>
          </dd>
        </dl>
      ...
        <dl>
        	<dt class="ask">...</dt>
        	<dd class="message">...</dd>
        	<dt class="ans">...</dt>
        	<dd class="nnl">...</dd>
        </dl>
       ...
       </dd>
       ...
    </dl></code>





Ahora realmente ud. pensara que yo estoy muy loco, ¿Como se le ocurre hacer una anidamiento de listas de definiciones?. En defensa propia y evitar que me "encasillen", me remito a la especificacion.




> Otra aplicación de DL es, por ejemplo, dar formato a un diálogo, de modo que cada DT identifica al hablante, y cada DD contiene sus palabras.




El elemento `dd id="feedback"` va a contener todas las preguntas/respuestas hechas/dadas a/por Carlos Tori por/a sus fieles lectores, esa seria la descripcion del termino: **Feedback: Los lectores preguntaron...**. Ahora bien, dentro del elemento `dd id="feedback"` nos encontraremos con varias listas de definiciones, la cuales mostraran los distintos "dialogos" que mantiene Carlos Tori con sus lectores. Se han marcado debidamente los distintos elementos.





dt class="ask"

    Identificamos al lector, en nuestro caso indicamos sus direcciones de correo electronico.

dd class="message"

    Indicamos el mensaje del lector, adicionalmente se coloca el **Asunto** y a quien va dirijido el mensaje (aunque sea obvio).

dt class="ans"

    Identifica a Carlos Tori, en nuestro caso indicamos su direccion de correo electronico.

dd class="nnl"

    Indicamos la respuesta dada por Carlos Tori al lector.




Ya para finalizar esta primera entrega, mejoramos el pie de pagina y le agregamos un contenedor a nuestro documento.




Mejora en el pie de pagina:



    
    <code><div id="footer">
      <p><em>Carlos Tori</em><br />
      <a href="http://www.wedoit.com.ar/">WedoIT</a> - NNL Newsletter<br />
      <acronym title="Pretty Good Privacy" xml:lang="en">PGP</acronym> ID 0x7F81D818</p>
      <p>Feedback, trabajo, sponsors, notas, contribuciones, dirigirse a: 
      <a href="mailto:soporte@gmail.com">
      soporte@gmail.com</a>.</p>
    </div></code>




Agregamos un contenedor al documento:



    
    <code><body>
      <div id="wrapper">
        ...
      </div>
    </body></code>




Vea una [muestra de los cambios hechos al documento](http://blog.milmazz.com.ve/wp-content/nnl/estructura.html).




En la proxima entrega vendra lo divertido para muchos, mejorar la presentacion del documento a traves de hojas de estilos en cascada o CSS.




#### Nota:




Este artículo lo publique el día 29 de Marzo de este mismo año en [BdW](http://www.bitacoradewebmaster.com/).
  *[ud.]: usted
  *[P.ej.]: Por ejemplo
