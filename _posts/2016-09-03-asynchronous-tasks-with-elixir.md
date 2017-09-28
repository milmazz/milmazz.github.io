---
author: milmazz
comments: true
share: true
date: 2016-09-03 08:00:00
layout: post
slug: asynchronous-tasks-with-elixir
title: 'Asynchronous Tasks with Elixir'
tags:
- elixir
- programming
---

One of my firsts contributions into [ExDoc][], the tool used to produce HTML
documentation for Elixir projects, was to improve the documentation build
process performance. My first approach for this was to build each module page
concurrently, manually sending and receiving messages between processes. Then,
as you can see in the Pull Request details, [Eric Meadows-Jönsson][@ericmj]
pointed out that I should look at the [Task][] module. In this article, I'll try
to show you the path that I followed to do that contribution.

The original source code was something like this:

```elixir
def run(modules, config) do
  # ...
  generate_list(modules, all, output, config, has_readme)
  generate_list(exceptions, all, output, config, has_readme)
  generate_list(protocols, all, output, config, has_readme)
  # ...
end

defp generate_list(nodes, all, output, config, has_readme) do
  Enum.each nodes, &generate_module_page(&1, all, output, config, has_readme)
end

defp generate_module_page(node, modules, output, config, has_readme) do
  content = Templates.module_page(node, config, modules, has_readme)
  File.write("#{output}/#{node.id}.html", content)
end
```

You can see that we can improve the build performance if we generate each module
page concurrently. So, let's do that in a moment!

For the purposes of this article, let me simplify the example above. So, please
assume that the following was the original piece of code:

```elixir
# source: demo.exs
defmodule AsyncTaskDemo do
  def run(nodes, output) do
    if File.exists? output do
      File.rm_rf! output
    end
    File.mkdir_p! output

    generate_list(nodes, output)
  end

  defp generate_list(nodes, output) do
    Enum.each nodes, &generate_module_page(&1, output)
  end

  defp generate_module_page(node, output) do
    name = String.capitalize(node)
    content = EEx.eval_string "Hello <%= name %>", [name: name]
    File.write("#{output}/#{node}.txt", content)
  end
end
```

As a second step, lets set up our test suite, in this case, we want to test a
single file `demo.exs`.

```elixir
# source: async_test.exs
ExUnit.start()

Code.require_file("demo.exs", __DIR__)

defmodule AsyncTaskDemoTest do
  use ExUnit.Case

  test "generate node pages" do
    nodes = ["john", "jane"]
    output = "doc"
    AsyncTaskDemo.run(nodes, output)

    files = File.ls! output

    assert files == ["jane.txt", "john.txt"]

    result = for f <- files do
       File.read! Path.join(output, f)
    end

    assert result == ["Hello Jane", "Hello John"]
  end
end
```

If we run our test suite we can see that everything is right:

```bash
$ elixir async_test.exs
.

Finished in 0.1 seconds (0.07s on load, 0.07s on tests)
1 test, 0 failures

Randomized with seed 114000
```

Ok, now it's time to introduce the concept of asynchronous tasks with
[`Kernel.spawn/1`][spawn/1]:

```elixir
defp generate_list(nodes, output) do
  Enum.each nodes, &generate_module_page_async(&1, output)
end

defp generate_module_page_async(node, output) do
  spawn(fn ->
    generate_module_page(node, output)
  end)
end

defp generate_module_page(node, output) do
  # ...
end
```

At this point, you'll notice that now `generate_list/2` calls a new function
that we named `generate_module_page_async/2`, this function will spawn new
processes, each process will generate a module page.

One problem with the earlier approach is that our program is not waiting for the
results of each invocation of the `generate_module_page/2` function. Basically,
we're doing a *fire and forget* concurrent execution, this means that the caller
process doesn't receive any feedback from the spawned function. If we run our
test we'll see that is failing:

```bash
$ elixir async_test.exs


  1) test generate node pages (AsyncTaskDemoTest)
     async_test.exs:8
     Assertion with == failed
     code:  files == ["jane.txt", "john.txt"]
     left:  []
     right: ["jane.txt", "john.txt"]
     stacktrace:
       async_test.exs:15: (test)



Finished in 0.07 seconds (0.05s on load, 0.02s on tests)
1 test, 1 failure

Randomized with seed 47515
```

We can fix this error doing the following:

```elixir
# source: demo.exs
  defp generate_list(nodes, output) do
    nodes
    |> Enum.map(&generate_module_page_async(&1, output))
    |> Enum.map(fn _ ->
      receive do
        :ok -> :ok
      end
    end)
  end

  defp generate_module_page_async(node, output) do
    caller = self()
    spawn(fn ->
      send(caller, generate_module_page(node, output))
    end)
  end

  defp generate_module_page(node, output) do
    # ...
  end
```

Let's run our tests:

```bash
$ elixir async_test.exs
.

Finished in 0.09 seconds (0.06s on load, 0.03s on tests)
1 test, 0 failures

Randomized with seed 474778
```

Until now, we're assuming that the [`File.write/3`][write/3] always returns
`:ok`. If for some reason [`File.write/3`][write/3] returns an `{:error,
reason}` message we'll get stuck. One way to solve this issue is by doing the
following:

```elixir
# source: demo.exs
  defp generate_list(nodes, output) do
    nodes
    |> Enum.map(&generate_module_page_async(&1, output))
    |> Enum.map(fn _ ->
      receive do
        :ok -> :ok
        {:error, reason} -> IO.puts :stderr, "#{reason}"
      end
    end)
  end
```

Finally, if we don't receive any message at all, we set a timeout after 5
seconds:

```elixir
  defp generate_list(nodes, output) do
    nodes
    |> Enum.map(&generate_module_page_async(&1, output))
    |> Enum.map(fn _ ->
      receive do
        :ok -> :ok
        {:error, reason} -> IO.puts :stderr, "#{reason}"
      after 5000 ->
        IO.puts :stderr, "Timeout"
      end
    end)
  end
```

With all these changes, we're ready to send our Pull Request, but wait, there is
a better way to do this.

## Elixir way: Task Module

As I mentioned before at the beginning of this article, [Eric][@ericmj] pointed
out that I should look at the [Task][] module documentation, and he was
absolutely right, this module offers a really good abstraction and now it's
really easy to run simple processes.

Applying the `Task.async/1` to our earlier example we cut down our source code
to:

```elixir
defp generate_list(nodes, output) do
  nodes
  |> Enum.map(&Task.async(fn ->
       generate_module_page(&1, output)
     end))
  |> Enum.map(&Task.await/1)
end
```

`Task.async/1` creates a separate process that runs the `generate_module_page/2`
function, then, we collect each task descriptor (returned by `Task.async/1`),
which is passed as the first value to `Task.await/2`, this call waits for our
background process to finish and returns its value, in this case, the result of
`File.write/3`.

You may ask yourself, *how is it that with the concurrent version we can improve
the overall performance?*, well, that depends, first we need to take into
account that our *concurrent program* will take advantage of a *parallel
computer* (several processing units), if we run our program on a computer with
only one CPU core, then, parallelism cannot happen.

Assume for a moment that the `generate_module_page` function always takes more
than 2 seconds:

```elixir
  defp generate_module_page(node, output) do
    :timer.sleep(2000)
    name = String.capitalize(node)
    content = EEx.eval_string "Hello <%= name %>", [name: name]
    File.write("#{output}/#{node}.txt", content)
  end
```

Then, with the following code we can test the performance improvements using a
*parallel computer*:

```elixir
# performance.exs
Code.require_file("demo.exs", __DIR__)

nodes = ["egg", "bacon", "spam", "sausage", "beans", "brandy", "foo", "baz"]
output = "doc"

before = System.monotonic_time()

AsyncTaskDemo.run(nodes, output)

later = System.monotonic_time()
diff = later - before
seconds = System.convert_time_unit(diff, :native, :seconds)

IO.puts "Diff: #{seconds} seconds. #{diff} :native time unit"
```

The results are the following:

```bash
# Sequential
$ elixir performance.exs
Diff: 16 seconds. 16122888704 :native time unit
# concurrent
$ elixir performance.exs
Diff: 2 seconds. 2052834417 :native time unit
```

The result of our concurrent version is eightfold faster than the sequential
version :)

## Wrapping up

Is always good to know how concurrency works in Erlang & Elixir, where you can
create new lightweight processes with `spawn`, and then send/receive messages
to/from those processes, you can also use some abstractions given by OTP (*Open
Telecom Platform*), in general, that's the way you can accomplish concurrency in
Erlang, but sometimes, you want to run simple processes, something like
background jobs, in those cases, is good to know about the [Task][] module,
which is a really good Elixir abstraction that keep us isolated from the details
and let's concentrate on our goals.

As [José Valim][@josevalim] later tweeted, this was another entry on the "hard
things made easier with Elixir" series.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Another entry on the &quot;hard things made easier with Elixir&quot; series: <a href="https://t.co/luQ8gJaBpE">https://t.co/luQ8gJaBpE</a> :)</p>&mdash; José Valim (@josevalim) <a href="https://twitter.com/josevalim/status/611451815616507904">June 18, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

## References

* [Kernel.spawn/1][spawn/1]
* [Task module][Task]
* [File.write/3][write/3]

## Acknowledgments

Thank you to [José Valim][@josevalim], [Sebastián Magrí][@sebasmagri] and Ana
Rangel for reviewing drafts of this post.

[contrib]: https://github.com/elixir-lang/ex_doc/pull/227
[ExDoc]: https://github.com/elixir-lang/ex_doc
[Task]: http://elixir-lang.org/docs/stable/elixir/Task.html
[josevalim]: https://twitter.com/josevalim/status/611451815616507904
[@ericmj]: https://github.com/ericmj
[@josevalim]: https://github.com/josevalim
[spawn/1]: http://elixir-lang.org/docs/stable/elixir/Kernel.html#spawn/1
[write/3]: http://elixir-lang.org/docs/stable/elixir/File.html#write/3
[@sebasmagri]: https://github.com/sebasmagri
