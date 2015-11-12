---
redirect_from: "/archivos/2013/09/23/erlang-primeras-impresiones/"
author: milmazz
comments: true
date: 2013-09-23 22:22:20
modified: 2014-02-07
layout: page
slug: elixir-primeras-impresiones
title: 'Elixir: Primeras impresiones'
tags: [programming, elixir]
share: true
---

### Elixir: Primeras impresiones

**NOTA: Este artículo originalmente lo escribí para [La Cara Oscura del Software](http://lacaraoscura.com/post/59947279510/elixir-primeras-impresiones), un *blog* colectivo dedicado a desarrollo de software.**

![](http://media.tumblr.com/67428e8bd65cf2747d8a1b19f67bb3b6/tumblr_inline_msfr2tm7tU1qz4rgp.png) Durante el segundo *hangout* de [@rubyVE](http://www.twitter.com/Rubyve) escuché a [@edgar](http://www.twitter.com/edgar) comentar sobre [Elixir][] y en verdad me llamó la atención lo que indicaba, siempre me inquieta conocer al menos un poco sobre otros lenguajes de programación, siempre terminas aprendiendo algo, una buena lección es seguro, sobre todo por aquello de la filosofía del programador pragmático y la necesidad de invertir regularmente en tu [portafolio de conocimientos][pragmatictips].

Ahora bien, después de leer un artículo de [Joe Armstrong][], padre de [Erlang][], en donde afirmaba que tras una semana de haber usado Elixir estaba **completamente entusiasmado** por lo visto. Con esto era claro que se estaba presentando para mi una gran oportunidad para retomar la programación funcional con [Elixir][], inicié con [Haskell][] en la Universidad en la materia de *Compiladores* y la verdad es que no lo he vuelto a tocar. 

[José Valim](http://www.twitter.com/josevalim) es un brasileño, parte del equipo [core committer](http://rubyonrails.org/core) de Rails. Después de sufrir [RSI][], en su afán por encontrar qué hacer en su reposo se puso a leer el libro: [Seven Languages in Seven Weeks: A Pragmatic Guide to Learning Programming Languages][7languages] y allí conoció [Erlang][] y su EVM (*Erlang Virtual Machine*), cierto tiempo después creo este nuevo lenguaje llamado Elixir, en donde uno de sus mayores activos es la EVM, tanto es así que de hecho no existe un costo de conversión al invocar Erlang desde Elixir y viceversa. Todo esto es seguramente es la respuesta de Valim a las limitantes físicas actuales en los procesadores o lo que se conoce también como: "[se nos acabó el almuerzo gratis][free-lunch-is-over]", sobre todo ahora con recientes anuncios de [Parallella][] y de Intel con los procesadores [Xeon Phi][].

A pesar de la horrible sintaxis de Erlang, o al menos después de leer a Damien Katz, autor original de [CouchDB](http://couchdb.apache.org/) en [What Sucks About Erlang](http://damienkatz.net/2008/03/what_sucks_abou.html) y a Tony Arcieri, autor de [Reia](http://reia-lang.org/) (otro lenguaje basado en BEAM), en su artículo [The Trouble with Erlang (or Erlang is a ghetto)](http://www.unlimitednovelty.com/2011/07/trouble-with-erlang-or-erlang-is-ghetto.html) es fácil concluir que la sintaxis no es la más amenas de todas. Sin embargo, las inmensas habilidades que brinda Erlang para establecer sistemas concurrentes (distribuidos, tolerantes a fallas y *code swapping*) ha permitido llegar a mantener hasta [2 millones de conexiones TCP en un solo nodo][2millions]. Es por ello que compañías como [Whatsapp][], [Facebook][], [Amazon][], [Ericsson][], [Motorola][], [Basho][] ([Riak][]) y [Heroku][] por mencionar algunas están usando Erlang para desarrollar sus sistemas.

Rápidamente quisiera compartirles mi felicidad por haber iniciado a explorar este lenguaje. Para iniciar tu proyecto tienes un magnífico utilitario llamado [mix][] (inspirado en [Leiningen][] de Clojure). Mix también permite manejar las tareas más comunes como administración de dependencias de tu proyecto, compilación, ejecución de pruebas, despliegue (pronto), entre otras. Incluso puedes programar nuevas tareas, simplemente asombroso, en fin, vamos a jugar:

    $ mix help
    mix                 # Run the default task (current: mix run)
    mix archive         # Archive this project into a .ez file
    mix clean           # Clean generated application files
    mix cmd             # Executes the given command
    mix compile         # Compile source files
    mix deps            # List dependencies and their status
    mix deps.clean      # Remove the given dependencies' files
    mix deps.compile    # Compile dependencies
    mix deps.get        # Get all out of date dependencies
    mix deps.unlock     # Unlock the given dependencies
    mix deps.update     # Update the given dependencies
    mix do              # Executes the tasks separated by comma
    mix escriptize      # Generates an escript for the project
    mix help            # Print help information for tasks
    mix local           # List local tasks
    mix local.install   # Install a task or an archive locally
    mix local.rebar     # Install rebar locally
    mix local.uninstall # Uninstall local tasks or archives
    mix new             # Creates a new Elixir project
    mix run             # Run the given file or expression
    mix test            # Run a project's tests

Procedamos con la creación de un nuevo proyecto:

    $ mix new demo
    * creating README.md
    * creating .gitignore
    * creating mix.exs
    * creating lib
    * creating lib/demo.ex
    * creating test
    * creating test/test_helper.exs
    * creating test/demo_test.exs

    Your mix project was created with success.
    You can use mix to compile it, test it, and more:

        cd demo
        mix compile
        mix test

    Run `mix help` for more information.

La estructura del proyecto creado por `mix` es como sigue:

    $ cd demo
    $ tree
    .
    |-- README.md
    |-- lib
    |   `-- demo.ex
    |-- mix.exs
    `-- test
        |-- demo_test.exs
        `-- test_helper.exs

    2 directories, 5 files

En `mix.exs` encontramos la configuración del proyecto así como sus dependencias en caso de aplicar, en `lib/demo.ex` ubicamos la definición del módulo que nos ayudará a estructurar posteriormente nuestro código, en `test/test_demo.exs` encontramos un esqueleto base para los casos de pruebas asociadas al modulo. Finalmente en `test/test_helper.exs` radica inicialmente el arranque del *framework* [ExUnit][].

Creemos un par de pruebas sencillas primero:

    $ vim test/demo_test.exs
    defmodule DemoTest do
      use ExUnit.Case

      test "factorial base case" do
        assert Demo.factorial(0) == 1
      end

      test "factorial general case" do
        assert Demo.factorial(10) == 3628800
      end

      test "map factorial" do
        assert Demo.map([6, 8, 10], fn(n) -> Demo.factorial(n) end) == [720, 40320, 3628800]
      end
    end

Evidentemente al hacer "mix test" todas las pruebas fallaran, vamos a comenzar a subsanar eso:

    $ vim lib/demo.ex
    defmodule Demo do

    def factorial(0) do
        1
    end

    def factorial(n) when n > 0 do
        n * factorial(n - 1)
    end

    end

En el par de bloques de código mostrado previamente se cubren los dos casos posibles del factorial.

Volvamos a correr las pruebas:

    $ mix test       
    ..

      1) test map factorial (DemoTest)
         ** (UndefinedFunctionError) undefined function: Demo.map/2
         stacktrace:
           Demo.map([6, 8, 10], #Function<0.60019678 in DemoTest.test map factorial/1>)
           test/demo_test.exs:13: DemoTest."test map factorial"/1



    Finished in 0.04 seconds (0.04s on load, 0.00s on tests)
    3 tests, 1 failures

De las 3 pruebas programadas hemos superado dos, nada mal, continuemos, volvamos a editar nuestro módulo:

    $ vim lib/demo.ex
    defmodule Demo do
        def factorial(0) do
            1
        end

        def factorial(n) when n > 0 do
            n * factorial(n - 1)
        end

        def map([], _func) do
            []
        end

        def map([head|tail], func) do
            [func.(head) | map(tail, func)]
        end
    end

En esta última versión se ha agregado la función `map`, básicamente esta función recibe una colección de datos y una función que se aplicará sobre cada uno de los elementos de la colección, para nuestros efectos prácticos la función que será pasada a `map` será el factorial.

Como nota adicional, los bloques de código vistos en el ejemplo anterior prefiero expresarlos de manera sucinta así, cuestión que también es posible en Elixir:

    $ vim lib/demo.ex
    defmodule Demo do
        @moduledoc """
        Demo module documentation, Python *docstrings* inspired.
        """
        def factorial(0), do: 1

        def factorial(n) when n > 0, do: n * factorial(n - 1)

        def map([], _func), do: []

        def map([head|tail], func), do: [func.(head) | map(tail, func)]
    end

Acá se pueden apreciar conceptos como *pattern matching*, *guard clauses*, manejo de listas y *docstrings* (inspirado en Python). Atención, los *docstrings* soportan [MarkDown][], junto a [ExDoc][] es posible producir sitios estáticos que extraen los *docstrings* a partir del código fuente.

Comprobemos los casos desde la consola interactiva `iex` antes de pasar de nuevo al caso automatizado:

    $ iex lib/demo.ex
    Erlang R16B01 (erts-5.10.2) [source] [64-bit] [smp:4:4] [async-threads:10] [hipe] [kernel-poll:false]

    Interactive Elixir (0.10.2-dev) - press Ctrl+C to exit (type h() ENTER for help)
    iex(1)> import Demo
    nil
    iex(2)> h(Demo)
    # Demo

    Demo module documentation, Python *docstrings* inspired.

    iex(3)> Demo.factorial(10)
    3628800
    iex(4)> Demo.map([6, 8, 10], Demo.factorial(&1))
    [720, 40320, 3628800]
    
Lo previo es una *consola interactiva*, vimos la documentación e hicimos unas pruebas manuales.

Seguro notaron que al final del ejemplo previo, al hacer el `map` he cambiado la forma en la que invoco a la función anónima la cual originalmente fue definida en las pruebas como `fn(n) -> Demo.factorial(n) end`, solamente he recurrido a un modo que permite Elixir y otros lenguajes funcionales para expresar este tipo de funciones de manera concisa, se le conoce como *Partials*.

Ahora corramos las pruebas automatizadas de nuevo:

    $ mix test
    Compiled lib/demo.ex
    Generated demo.app
    ...

    Finished in 0.04 seconds (0.04s on load, 0.00s on tests)
    3 tests, 0 failures

Con eso hemos pasado los casos de pruebas.

En este caso particular, prefiero que las pruebas sean autocontenidas en el módulo, además, no recurrimos a *fixtures* ni nada por el estilo, así que vamos a cambiar el código para que soporte *doctest*

    $ vim lib/demo.ex
    defmodule Demo do
        @moduledoc """
        Demo module documentation, Python *docstrings* inspired.
        """

        @doc """
        Some examples

        iex> Demo.factorial(0)
        1

        iex> Demo.factorial(10)
        3628800

        iex> Demo.map([6, 8, 10], Demo.factorial(&1))
        [720, 40320, 3628800]
        """
        def factorial(0), do: 1

        def factorial(n) when n > 0, do: n * factorial(n - 1)

        def map([], _func), do: []

        def map([head|tail], func), do: [func.(head) | map(tail, func)]
    end

Dado lo anterior ya no es necesario tener las pruebas aparte, por lo que reduzco:

    $ vim test/demo_test.exs
    defmodule DemoTest do
      use ExUnit.Case
      doctest Demo
    end

Comprobamos la equivalencia:

    $ mix test              
    ...

    Finished in 0.06 seconds (0.06s on load, 0.00s on tests)
    3 tests, 0 failures

Simplemente hermoso, cabe resaltar que lo mencionado es solo rascar un poco la superficie de Elixir :-)

Ah, por cierto, ya para finalizar, José Valim está apuntando el desarrollo de Elixir y [Dynamo][] (framework) a la Web, lo ha dejado claro, por eso he visto que algunos programadores Rails están "echándole un ojo" a Elixir, al menos eso es lo que concluyo de los [elixir-issues][] en Github, el reciente screencast de [Peepcode][] (vale la pena comprarlo) y los libros que se avecinan de [Dave Thomas][PragProg] y [Simon St. Laurent][OReilly].

Quizá en una nueva oportunidad hablemos de Macros, pase de mensajes entre procesos, Protocolos (inspirados en [Clojure protocols](http://clojure.org/protocols)), Reducers (inspirados también en [Clojure Reducers](http://clojure.com/blog/2012/05/08/reducers-a-library-and-model-for-collection-processing.html)), HashDict, el hermoso y *nix like operador pipeline (`|>`), mejorar nuestra implementación de la función `map` para que haga los cálculos de manera concurrente, entre otros.

Espero hayan disfrutado la lectura, que este artículo sirva de abreboca y les anime a probar Elixir.

[Elixir]: http://www.elixir-lang.org
[pragmatictips]: http://pragmatictips.com/8
[Joe Armstrong]: http://joearms.github.io/2013/05/31/a-week-with-elixir.html
[Haskell]: http://www.haskell.org
[RSI]: http://en.wikipedia.org/wiki/Repetitive_strain_injury
[7languages]: http://pragprog.com/book/btlang/seven-languages-in-seven-weeks
[free-lunch-is-over]: http://www.gotw.ca/publications/concurrency-ddj.htm
[Parallella]: http://www.parallella.org/
[Peepcode]: https://peepcode.com/products/elixir
[PragProg]: http://pragprog.com/book/elixir/programming-elixir
[OReilly]: http://shop.oreilly.com/product/0636920030584.do
[mix]: http://elixir-lang.org/getting_started/mix/1.html
[Leiningen]: https://github.com/technomancy/leiningen
[Erlang]: http://www.erlang.org/
[Xeon Phi]: http://www.drdobbs.com/parallel/intels-50-core-xeon-phi-the-new-era-of-i/240105810
[2millions]: http://blog.whatsapp.com/index.php/2012/01/1-million-is-so-2011/
[Whatsapp]: http://www.whatsapp.com/
[Facebook]: http://www.facebook.com/
[Amazon]: http://www.amazon.com/
[Ericsson]: http://www.ericsson.com/‎
[Motorola]: http://www.motorola.com/‎
[Heroku]: https://www.heroku.com/
[Riak]: http://basho.com/riak/
[Basho]: http://basho.com/erlang-at-basho-five-years-later/
[Dynamo]: https://github.com/elixir-lang/dynamo
[elixir-issues]: https://github.com/elixir-lang/elixir/issues/1663
[ExDoc]: https://github.com/elixir-lang/ex_doc
[MarkDown]: http://daringfireball.net/projects/markdown/
[ExUnit]: http://elixir-lang.org/getting_started/ex_unit/1.html
