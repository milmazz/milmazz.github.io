---
redirect_from: "/archivos/2006/06/13/el-algoritmo-de-dijkstra-en-perl/"
author: milmazz
comments: true
date: 2006-06-13 17:10:22
layout: post
slug: el-algoritmo-de-dijkstra-en-perl
title: El algoritmo de Dijkstra en Perl
categories:
- Perl
tags:
- dijkstra
- graph
- graphviz
- Perl
---

Hace ya algunos días nos fué asignado en la cátedra de _Redes de Computadoras_ encontrar el camino más corto entre dos vértices de un grafo dirigido que **no** tuviese costos negativos en sus arcos, para ello debíamos utilizar el [algoritmo de Dijkstra](http://en.wikipedia.org/wiki/Dijkstra%27s_algorithm). Además de ello, debía presentarse el grafo y la ruta más corta en una imagen, para visualizarlo de mejor manera.

Cuando un profesor te dice: No se preocupen por la implementación del algoritmo de Dijkstra, utilice la que usted prefiera, pero les agradezco que la analicen. Además, pueden utilizar el lenguaje de programación de su preferencia. Estas palabras te alegran el día, simplemente eres **feliz**.

Por algunos problemas que tuve con algunos módulos en Python, no lo hice en dicho lenguaje de programación, no vale la pena explicar en detalle los problemas que se me presentaron.

Lo importante de todo esto, es que asumí el reto de realizar la asignación en un lenguaje de programación practicamente nuevo para mí, aunque debo reconocer que la resolución no me causó dolores de cabezas en lo absoluto, a continuación algunos detalles.

En primer lugar, formularse las preguntas claves, ¿existe algún módulo en Perl que implemente el algoritmo de Dijkstra para encontrar el camino más corto?, ¿existe algún módulo en Perl que implemente [GraphViz](http://graphviz.org/)?

En segundo lugar, buscar en el lugar correcto, y cuando hablamos de Perl el sitio ideal para buscar es [search.cpan.org](http://search.cpan.org/), efectivamente, en cuestión de **segundos** encontre los dos módulos que me iban a facilitar la vida, `Bio::Coordinate::Graph`, para encontrar la ruta más corta entre dos vértices en un grafo y `GraphViz`, para pintar el grafo.

En tercer lugar, verificar en tu distro favorita, Debian, si existe un paquete precompilado listo pasa ser usado, para GraphViz existe uno, `libgraphviz-perl`, así que para instalarlo es tan fácil como:

    # aptitude install libgraphviz-perl

Para instalar el módulo `Bio::Coordinate::Graph`, primero debía _debianizarlo_, eso es tan sencillo como hacer.

    # dh-make-perl --build --cpan Bio::Coordinate::Graph

Luego debe proceder a instalar el fichero `.deb` que se generó con `dh-make-perl`, para lograrlo hago uso del comando `dpkg`, eso es todo. Por cierto, acerca del comando `dh-make-perl` ya había hablado en la entrada [Perl: Primeras experiencias](/archivos/2006/05/17/perl-primeras-experiencias/).

## Encontrando el camino más corto

Según dice la documentación del módulo `Bio::Coordinate::Graph` debemos hacer uso de un _hash_ de _hashes_ anónimos para representar la estructura del grafo, en ese momento recordé algunas palabras que José Luis Rey nos comentó en la primera parte del curso de Perl, vale resalta la siguiente: Si usted no utiliza un _hash_, no lo está haciendo bien.

{% highlight perl %}
my $hash = {
		'6' => undef,
		'1' => {
			'2' => 1
			},
		'2' => {
			'6' => 4,
			'4' => 1,
			'3' => 1
			},
		'3' => {
			'6' => 1
			},
		'4' => {
			'5' => 1
			},
		'5' => undef
	};
{% endhighlight %}

Algo que es importante saber es que en Perl las estructuras de datos son planas, lo cual es conveniente. Por lo tanto, vamos a tener que utilizar _referencias_ en este caso, pero luego nos preocuparemos por ello. Ahora solo resta decir que, la estructura hecha a través de un _hash_ de _hashes_ es sencilla de interpretar, las _llaves_ o _keys_ del _hash_ principal representan los vértices del grafo, ahora bien, las _llaves_ de los hashes más internos representan los vértices vecinos de cada vértice descrito en el hash principal, el valor de los hashes más internos representa el _peso_, distancia o costo que existe entre ambos vértices, para aclarar la situación un poco: El vértice **2**, tiene como vecinos a los vértices **3**, **4** y **6**, con costos de **1**, **1** y **4** respectivamente. Puede ver una muestra del [grafo resultante](http://blog.milmazz.com.ve/wp-content/grafo-orig.ps).

De manera alternativa puede utilizar un _hash_ de _arrays_ para representar la estructura del grafo, siempre y cuando **todos** los costos entre los vértices sean igual a **1**, este método es menos general que el anterior y además, este tipo de estructura es convertida a un _hash_ de _hashes_, así que a la final resulta ineficiente.

Una vez definida la estructura del grafo corresponde crear el objeto, esto es realmente sencillo.

{% highlight perl %}
my $graph = Bio::Coordinate::Graph->new(-graph => $hash);
{% endhighlight %}

Lo que resta es definir el vértice de inicio y el vértice final, yo los he definido en las variables `$start` y `$end`, luego de ello, debemos _invocar_ al método `shortest_path`, de la siguiente manera:

{% highlight perl %}
my @path = $graph->shortest_path($start, $end);
{% endhighlight %}

En el _array_ `@path` encontraremos los vértices involucrados en el camino más corto.

## Pintando el grafo

Una vez hallado el camino más corto entre dos vértices dados en un grafo dirigido con un costo en los arcos siempre positivos, lo que resta es hacer uso del módulo `GraphViz`, hacemos uso del constructor del objeto de la siguiente manera:

{% highlight perl %}
my $g = GraphViz->new();
{% endhighlight %}

Usted puede _invocar_ al constructor con distintos atributos, para saber cuales usar le recomiendo leer la documentación del módulo.

Ahora bien, yo quería distinguir aquellos vértices involucrados en el camino más corto de aquellos que no pertenecían, así que lo más sencillo es generar una lista de atributos que será usada posteriormente. Por ejemplo:

{% highlight perl %}
my @shortest_path_attrs = (
				color => 'red',
			);
{% endhighlight %}

Una vez definidos los atributos, debemos generar cada uno de sus vértices y además, establecer los arcos entre cada uno de ellos. Para ello haremos uso de los métodos `add_node`, `add_edge`.

{% highlight perl %}
foreach my $key (keys %$hash){
	$g->add_node($key);
	foreach my $neighbor (keys %{$hash->{$key}}){
		$g->add_edge($key => $neighbor, label => $hash->{$key}->{$neighbor});
	}
}
{% endhighlight %}

El bloque de código anterior lo que hace es agregar cada uno de los vértices que se encuentran en el _hash_ que representa la estructura del grafo, para cada uno de estos vértices, se añade cada uno de los _arcos_ que lo conectan con sus vecinos, nótese el manejo del operador flecha (**->**) para desreferenciar las referencias al _hash_ de _hashes_ anónimos.

Una vez construidos los nodos y sus arcos, ¿cómo reconocer aquellos que pertenecen al camino más corto?, bueno, toda esa información la podemos extraer del _array_ que hemos denominado `@path`.

{% highlight perl %}
for (my $count=0; $count < = $#path; $count++){
	$g->add_node($path[$count], @shortest_path_attrs);
	$g->add_edge($path[$count] => $path[$count+1], @shortest_path_attrs) unless ($count+1 > $#path);
}
{% endhighlight %}

Según dice la documentación del módulo `GraphViz` todos los atributos de un vértice del grafo no tienen que definirse de una sola vez, puede hacerse después, ya que las declaraciones sucesivas tienen efecto acumulativo, eso quiere decir lo siguiente:

{% highlight perl %}
$g->add_node('1', color => 'red');
{% endhighlight %}

Es equivalente a hacer las siguientes declaraciones sucesivas.

{% highlight perl %}
$g->add_node('1');
$g->add_node('1', color => 'red');
{% endhighlight %}

Lo que resta por hacer en este instante es exportar el objeto creado, puede crear por ejemplo un PNG, texto sin formato, PostScript, entre otros.

Yo decidí generar un fichero PostScript:

{% highlight perl %}
$g->as_ps("dijkstra.ps");
{% endhighlight %}

Por supuesto, les dejo una [muestra de los resultados](http://blog.milmazz.com.ve/wp-content/dijkstra.ps). Los vértices cuyo borde es rojo, son aquellos involucrados en el camino más corto desde el vértice inicio al vértice final, los arcos que marcan la ruta más corta también han sido coloreados.

Como siempre, todas las recomendaciones, comentarios, sugerencias son bienvenidas.
