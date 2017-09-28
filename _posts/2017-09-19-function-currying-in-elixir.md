---
author: milmazz
comments: true
share: true
date: 2017-09-19 21:00:00
layout: post
slug: function-currying-in-elixir
title: 'Follow-up: Function currying in Elixir'
tags:
- elixir
- programming
---

> *NOTE:* This article is a follow-up examination after the blog post
> [Function currying in Elixir][1] by [@stormpat][2]

In his article, Patrik Storm, shows how to implement _function currying_ in
Elixir, which could be really neat in some situations. For those who haven't
read Patrik's post, first, let us clarify what is _function currying_.

*Currying* is the process of transforming a function that takes multiple
arguments (_arity_) into a function that takes _only one_ argument and returns
another function if any arguments are still required. When the last required
argument is given, the function automatically executes and computes the result.

As a first step, let us apply _function currying_ manually:

```elixir
iex(1)> greet = fn greeting, name -> IO.puts "#{greeting}, #{name}" end
#Function<12.52032458/2 in :erl_eval.expr/5>
iex(2)> greet.("Hello", "John") # uncurried function
Hello, John
:ok
iex(3)> greetCurry = fn greeting -> fn name -> IO.puts "#{greeting}, #{name}" end end
#Function<6.52032458/1 in :erl_eval.expr/5>
iex(4)> greetCurry.("Hello").("John")
Hello, John
:ok
```

To get a general solution, Patrik uses a nice approach that combines _pattern
matching_ and _tail-call optimization_, let's dive into his implementation:


```elixir
# file: curry.exs
defmodule Curry do
  def curry(fun) do
    {_, arity} = :erlang.fun_info(fun, :arity)
    curry(fun, arity, [])
  end

  def curry(fun, 0, arguments) do
    apply(fun, Enum.reverse arguments)
  end

  def curry(fun, arity, arguments) do
    fn arg -> curry(fun, arity - 1, [arg | arguments]) end
  end
end
```

The main points in this `Curry` module are the following:

* `Curry.curry/1` represents our entry point, this function use
  [`:erlang.func_info/2`][3] to know the arity (number of arguments) of the given
  function `fun`. Then, we pass the control to the function `Curry.curry/3`
* The recursive function `Curry.curry/3` will return _anonymous functions_ that
  only takes just one argument.
* When the last required argument is given we will use [`Kernel.apply/2`][4] to
  invoke the given function `fun` with the list of arguments `args`.

Let's show how we can use _function currying_, I'll use the same examples
that Patrik did in his post but using `ExUnit` instead:

```elixir
# file: curried.exs
defmodule Curried do
  import Curry

  def match term do
    curry(fn what -> (Regex.match?(term, what)) end)
  end

  def filter f do
    curry(fn list -> Enum.filter(list, f) end)
  end

  def replace what do
    curry(fn replacement, word ->
      Regex.replace(what, word, replacement)
    end)
  end
end
```

Our unit tests:

```elixir
# file curry_test.exs
ExUnit.start()

Code.require_file("curry.exs", __DIR__)
Code.require_file("curried.exs", __DIR__)

defmodule CurryTest do
  use ExUnit.Case

  test "applying all the params at once or one step at a time should produce same results" do
    curried = Curry.curry(fn a, b, c, d -> a * b + div(c, d) end)
    five_squared = curried.(5).(5)

    assert five_squared.(10).(2) == curried.(5).(5).(10).(2)
  end

  test "curry allow to create composable functions" do
    has_spaces = Curried.match(~r/\s+/)
    sentences = Curried.filter(has_spaces)
    disallowed = Curried.replace(~r/[jruesbtni]/)
    censored = disallowed.("*")

    allowed = sentences.(["justin bibier", "and sentences", "are", "allowed"])

    assert "****** ******" == allowed |> List.first() |> censored.()
  end
end
```

Now we can run our tests as follows:

```console
$ elixir curry_test.exs
..

Finished in 0.2 seconds (0.2s on load, 0.00s on tests)
2 tests, 0 failures

Randomized with seed 604000
```

It is working, but I feel we can improve a few things, in this case, our `curry`
function only takes into account that the arguments are given from left to
right. What about if we want to give the parameters from right to left? Let's
introduce `curryRight`:

```elixir
# file: curry.exs
defmodule Curry do
  def curry(fun) when is_function(fun), do: curry(fun, :left)

  def curryRight(fun) when is_function(fun), do: curry(fun, :right)

  defp curry(fun, direction) do
    {_, arity} = :erlang.fun_info(fun, :arity)
    curry(fun, arity, [], direction)
  end

  defp curry(fun, 0, args, :left) do
    apply(fun, Enum.reverse(args))
  end

  defp curry(fun, 0, args, :right) do
    apply(fun, args)
  end

  defp curry(fun, arity, args, direction) do
    &curry(fun, arity - 1, [&1 | args], direction)
  end
end
```

Then, our `Curried` module, which holds support functions, is much simpler if we
do the following:

```elixir
# file: curried.exs
defmodule Curried do
  import Curry

  def match(term), do: curry(&Regex.match?/2).(term)

  def filter(f), do: curryRight(&Enum.filter/2).(f)

  def replace(what), do: curry(&Regex.replace(&1, &3, &2)).(what)
end
```

Now, without any change in our unit tests, we can verify that everything is
working as before.

```console
$ elixir curry_test.exs
..

Finished in 0.2 seconds (0.2s on load, 0.00s on tests)
2 tests, 0 failures

Randomized with seed 561000
```

## Do we need to apply curry to everything?

No, it will always depend of your case, first, let's see how worse can be if
apply _currying_ manually and then we will try to find another way to this whole
process as a _data transformation workflow_.

```elixir
# file: manual_currying.exs
defmodule ManualCurrying do
  def match(term) do
    fn what -> Regex.match?(term, what) end
  end

  def filter(f) do
    fn list -> Enum.filter(list, f) end
  end

  def replace(what) do
    fn replacement ->
      fn word ->
        Regex.replace(what, word, replacement)
      end
    end
  end
end
```

Our unit tests:

```elixir
# file manual_currying_test.exs
ExUnit.start()

Code.require_file("manual_currying.exs", __DIR__)

defmodule MunualCurryingTest do
  use ExUnit.Case
  import ManualCurrying

  test "applying all the params at once or one step at a time should produce same results" do
    curried =
      fn a ->
        fn b -> 
          fn c ->
            fn d ->
              a * b + div(c, d)
            end
          end
        end
      end

    five_squared = curried.(5).(5)

    assert five_squared.(10).(2) == curried.(5).(5).(10).(2)
  end

  test "curry allow to create composable functions" do
    has_spaces = match(~r/\s+/)
    sentences = filter(has_spaces)
    disallowed = replace(~r/[jruesbtni]/)
    censored = disallowed.("*")

    allowed = sentences.(["justin bibier", "and sentences", "are", "allowed"])

    assert "****** ******" == allowed |> hd() |> censored.()
  end
end
```

But, if you just one to execute this just one time, maybe we can do better
thinking everything as a _data transformation workflow_, and actually this is
the more succint way:

```elixir
"****** ******" ==
  ["justin bibier", "and sentences", "are", "allowed"]
  |> Enum.filter(&Regex.match?(~r/\s+/, &1))
  |> hd()
  |> String.replace(~r/[jruesbtni]/, "*")
```

## Wrapping up

_Function currying_ is an interesting technique that allow us to reuse
functions, for example, we can create a module with small functions that behave
consistently without so much effort. Although, we need to keep in mind the
arguments order when we want to apply `function currying`. Sometimes for
functions like [`Enum.map/2`][5], [`Enum.reduce/2`][6], [`Enum.filter/2`][7],
etc. it would be better or easier to use `curryRight` than `curry`, normally
our decision will depend on the arguments that will change constantly, because
we want to put those at the end of the execution path.

As a final note, it could be a interesting exercise to implement `uncurry`,
which is a function that converts a _curried function_ to a function with
_arity_ *n*, that way we can convert these two types in either direction.

## References

* [Function currying in Elixir][1] by [Patrik Storm][2]
* [:erlang.fun_info/2][3]
* [Kernel.apply/2][4]

[1]: http://blog.patrikstorm.com/function-currying-in-elixir
[2]: https://twitter.com/stormpat
[3]: https://erlang.org/doc/man/erlang.html#fun_info-2
[4]: https://hexdocs.pm/elixir/master/Kernel.html#apply/2
[5]: https://hexdocs.pm/elixir/master/Enum.html#map/2
[6]: https://hexdocs.pm/elixir/master/Enum.html#reduce/2
[7]: https://hexdocs.pm/elixir/master/Enum.html#filter/2
