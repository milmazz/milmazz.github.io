---
title: "Oban: Testing your Workers and Configuration"
author: milmazz
tags:
  - programming
  - elixir
---

In this article, I will continue talking about Oban, but I'll focus on how to
test your workers and, also, your configuration.

<!--more-->

This article is the second one of a series about Oban, an Elixir library for
background job processing:

* [Oban: job processing library for Elixir](/article/2022/02/11/oban-job-processing-package-for-elixir/)
* Oban: Testing your Workers and Configuration (you are here)
* Oban in production

## Testing the implementation of the Oban.Worker behaviour

Before implementing an Oban Worker, I start writing the unit test that will
insert and execute the job, these tests are usually short because your workers
should be as lean as possible, I tend to treat `Workers` as `Resolvers` or
`Controllers` that orchestrate a series of actions that most of the time,
execute functions that are already being tested and are part of the business
logic layer. It would also help validate that the job is running the side
effects in these unit tests.

```elixir
defmodule MyApp.Migrations.MyDataMigrationWorkerTest do
  use ExUnit.Case, async: true 
  use Oban.Testing, repo: MyApp.Repo

  alias MyApp.Migrations.MyDataMigrationWorker 

  describe "perform/1" do
    test "performs data migration" do
      # prepare your test
      assert {:ok, result} = perform_job(MyDataMigrationWorker, %{"my_arg" => 1})
      # assert the side-effects of the Oban Worker
    end
  end
end
```

In the previous test, we used the
[`perform_job/2`](https://hexdocs.pm/oban/Oban.Testing.html#perform_job/3)
helper from the `Oban.Testing` module, this helper constructs a job and execute
it with the given worker module. Apart from reducing the ceremony when
constructing jobs for unit test, one of the neat things about `perform_job/2`
is that it does some assertions for us.

From the docs, we have the following:

> * That the worker implements the `Oban.Worker` behaviour
> * That the options provided build a valid job
> * That the return is valid, e.g. `:ok`, `{:ok, value}`, `{:error, value}`, etc.
>
> If all of the assertions pass, the function returns the result of `perform/1`
for you to make additional assertions.

Apart from `perform_job/2,3` there are other testing helpers such as:
`assert_enqueued/2,3`, `all_enqueued/2`, `refute_enqueued/2,3`. For more
information please check the [Oban.Testing][] documentation.

It is important to note that if you want to test a worker included in Oban Pro
like the [Batch][], [Chunk][], or [Workflow][], you should `import` the module
`Oban.Pro.Testing` and use the `process_job/2,3` helper instead:

```elixir
import Oban.Pro.Testing

alias MyApp.MyBatchWorker

describe "process/1" do
  test "archives the given thing" do
    # prepare your test
    assert :ok == process_job(MyBatchWorkerBatch, %{thing_id: thing.id})
    # assert the side-effects of the Oban Batch Worker
  end
end
```

One important thing about testing, don't forget to test your Oban
configuration. Let's talk about it next.

## Testing your Oban Configuration

Do not wait until you deploy your application into your _staging_ environment
to catch errors in your Oban configuration, try to catch those errors as soon
as possible; you can start by doing:

```elixir
defmodule MyApp.ObanConfigTest do
  use ExUnit.Case, async: true

  test "check oban configuration" do
    [config, prod] =
      Enum.map(["config/config.exs", "config/prod.exs"], fn path ->
        path
        |> Config.Reader.read!(imports: [], env: :prod, target: :host)
        |> get_in([:core, Oban])
      end)

    assert %Oban.Config{plugins: _plugins} =
             config
             |> Keyword.merge(prod)
             |> Oban.Config.new()
  end
end
```

But, for the previous unit test to make sense, you need to know a bit of the
internal implementation of Oban, well, the gist is this, when you try to start
the _supervision tree_ for Oban, it first tries to create an `Oban.Config`
struct via
[`Oban.Config.new/1`](https://github.com/sorentwo/oban/blob/fec80cdbafe59b0fba13c0f67ed1739c1f86e703/lib/oban/config.ex#L44-L63),
and if you supply a wrong configuration, it will blow up.

It is important to note that the `Oban.Config.new/1` function doesn't validate
the _internal configuration_ for the plugins; it only validates that the given
plugin is an `atom`, is loaded, the plugin in question exports an `init/1`
function, and also the given `opts` is a keyword list. We will explore a
workaround for this limitation in the following sub-section.

### Testing your plugins configuration

Previously I mentioned that you should test your Oban configuration. Still,
there were limitations in the previous approach; mainly, `Oban.config.new/1`
doesn't validate the _internal configuration_ that you pass to each plugin
listed under the `plugins` option.

```elixir
test "check oban configuration" do
  # ...
  assert %Oban.Config{plugins: _plugins} =
           config
           |> Keyword.merge(prod)
           |> Oban.Config.new()
end
```

So, if you want to go further and validate the given options to the
`Oban.Plugins.Cron` plugin, for example, you need to know a bit about the
internal implementation and use a combination of
`Oban.Plugins.Cron.validate!/1` and `Oban.Plugins.Cron.init/1` and then assert
that the returned `{:ok, state, _}` is what you expect.

I like to validate the `cron` plugin configuration because it constantly
evolves, and it's easy to catch typos in the cron worker module names also
validating the cron expressions by doing the following:

```elixir
assert %Oban.Config{plugins: plugins} =
         config
         |> Keyword.merge(prod)
         |> Oban.Config.new()

assert :ok = Oban.Plugins.Cron.validate!(cron_opts)

assert {:ok, _state, {:continue, :start}} = Oban.Plugins.Cron.init(cron_opts)
```

A word of caution about the previous unit test segment, regardless that
`Oban.Plugins.Cron.validate!/1` is a public function, it has a `@doc false`,
which usually means that function is for internal use. There could be
unannounced breaking changes in the future, but you gain the following checks:

* that the value for the `crontab` option is a list.
* if you use the `timezone` option, it checks that the given value it's a known timezone

Regarding the usage of `Oban.Plugins.Cron.init/1`, you need to know that every
Oban plugin must implement an `init/1` function. Most of the time, the plugins
follow the `GenServer` behaviour, and in the specific case of the cron plugin,
the `init/1` parse the crontab expressions; if an expression is invalid, the
server initialization via `init/1` will fail. But, on the other hand, it also
validates that the given worker is valid, so this will catch possible typos.

I think it is worth taking the risk of using a _non-documented public function_
in this case; you gain more in your daily routine because it will allow you to
catch errors in your Oban Configuration as soon as possible.

After talking with Parker about the previous challenges, he suggested opening
the following issue in the Oban repository: [Improve the testing experience for
Oban and its plugins](https://github.com/sorentwo/oban/issues/632)

## Testing workers included in Oban Pro

In a previous section, I mentioned that when you're testing a worker included
in Oban Pro, like the [Batch][], [Chunk][], or [Workflow][], you should
`import` the module `Oban.Pro.Testing` and use the `process_job/2,3` helper
instead:

Possibly, the most challenging worker to test is the BatchWorker, especially if
you want to try the whole cycle, including their _handle callbacks_, but let's
start with some dummy Batch Worker. Then we can start talking about the details
of its tests.

So, let's imagine the following worker:

```elixir
defmodule MyApp.MyBatchWorker do
  use Oban.Pro.Workers.Batch,
    queue: :default

  require Logger

  @impl Batch
  def process(_) do
    :ok
  end

  @impl Batch
  def handle_exhausted(%Job{} = _job) do
    Logger.info("exhausted callback")
  end
end
```

The body of the module `MyApp.ByBatchWorker` is very similar to the usual
`Oban.Worker`. The exception is that you need to handle your work under
`process/1` instead of the `perform/1`; the latter is used internally. One of
the excellent additions of the `Oban.Pro.Workers.Batch` behaviour allows you to
define a few callbacks that are perfectly explained in the [behaviour
documentation][Batch]. In this example, I'm using the `handle_exhausted`
callback, called after _all jobs_ in the batch have either a `completed` or
`discarded` state. You also see that I'm just logging that the callback was
called in this case to keep the example as simple as possible, but you can do
whatever you want here.

Okay, now the exciting part, let's see how to test this _worker_.

```elixir
defmodule MyApp.MyBatchWorkerTest do
  use ExUnit.Case

  import Oban.Pro.Testing

  alias MyApp.MyBatchWorker

  describe "process/1" do
    test "processes the background job" do
      assert :ok = process_job(MyBatchWorker, %{})
    end
  end
end
```

Here we're testing the "bulk" of the worker. Now, let's test one of the handler
callbacks as a unit:

```elixir
  use Oban.Testing, repo: MyApp.Repo
  
  describe "handle_exhausted/1" do
    test "handle exhausted behaves correctly" do
      assert :ok = perform_job(MyBatchWorker, %{}, meta: %{"callback" => "exhausted"})
    end
  end
```

As far as I know, the test helper `process_job/2,3` will not work in the
previous test because it ends calling our `MyApp.MyBatchWorker.process/1`, and
we actually want to test our `MyApp.MyBatchWorker.perform/1`, which coordinates
the execution of the callbacks. Again, here you need to know how Oban works
internally. That's why I ended up using: `perform_job/3`, passing the `meta`
option. But please, be aware that `perform_job/3` will not complain if your
handle callback last expression returns things like `{:ok, value}`, `{:error,
value}`, etc. Please remember that the handle callbacks contract specifies that
you must return `:ok`. A possible solution that is being considered is to
include a `perform_callback/3` helper under the `Oban.Pro.Testing` module.

Now, let's assume that we want to go further in our test and check the whole
cycle. In that case, we do:

```elixir
defmodule MyApp.MyBatchWorkerTest do
  use ExUnit.Case

  import ExUnit.CaptureLog
  
  alias MyApp.MyBatchWorker

  require Logger
  
  # ...

  setup do
    oban_name = start_supervised_oban!(queues: [default: 3])
    Logger.put_module_level(MyBatchWorker, :all)

    on_exit(fn ->
      Logger.delete_module_level(MyBatchWorker)
    end)

    %{oban_name: oban_name}
  end

  test "testing our batch worker", %{oban_name: oban_name} do
    batch =
      ["foo@example.com", "bar@example.com"]
      |> Enum.map(&%{email: &1})
      |> MyBatchWorker.new_batch()

    log =
      capture_log(fn ->
        Oban.insert_all(oban_name, batch)
        Oban.drain_queue(queue: :default)
      end)
      
    assert log =~ "exhausted callback"
  end

  defp start_supervised_oban!(opts) do
    default_opts = [
      name: make_ref(),
      repo: MyApp.Repo,
      plugins: [
        {Oban.Pro.Plugins.BatchManager, debounce_interval: 5},
        {Oban.Plugins.Repeater, interval: 25}
      ],
      poll_interval: :timer.minutes(10),
      shutdown_grace_period: 25
    ]

    opts = Keyword.merge(default_opts, opts)
    name = opts[:name]

    start_supervised!({Oban, opts})

    name
  end
end
```

Wow, I know this is a lot to digest at once, so let's split the previous
snipped.

Suppose you saw the talk [Testing Oban Jobs From the Inside Out][oban-talk]
from Parker. In that case, you remember that he calls this kind of testing
"_inline testing_", and this should be used when you absolutely must normally
run jobs during your test (e.g., LiveView, Browser Testing), or in my case, I
wanted to test the whole path in my _Batch Workers_. So, let's start analyzing
the `start_supervised_oban!/1` helper:

```elixir
  defp start_supervised_oban!(opts) do
    default_opts = [
      name: make_ref(),
      repo: MyApp.Repo,
      plugins: [
        {Oban.Pro.Plugins.BatchManager, debounce_interval: 5},
        {Oban.Plugins.Repeater, interval: 25}
      ],
      poll_interval: :timer.minutes(10),
      shutdown_grace_period: 25
    ]

    opts = Keyword.merge(default_opts, opts)
    name = opts[:name]

    start_supervised!({Oban, opts})

    name
  end
```

Here, with the help of [`start_supervised!/2`][start_supervised], which comes
with [ExUnit][], we're setting up an Oban supervisor with some default options;
the essential thing in the previous snippet is the plugins. So first,
`Oban.Pro.Plugins.BatchManager` is needed because that's the plugin that will
track the execution of the Oban jobs within a batch, and it will also enqueue
the callback jobs. So, yes, the callbacks are also Oban Jobs.

Then, we have the [`Oban.Plugins.Repeater`][Repeater] plugin, which will poll
every 25 milliseconds to look for new jobs. This plugin is essential in our
case because inside the unit tests, PostgreSQL notifications don't work. We're
using the Ecto Sandbox, which means that every unit test runs inside a
transaction. Also, note that I'm using `make_ref` to create a unique reference
that we will use as the name for the Oban supervisor; you cannot have more than
one supervisor with the same `id`.

```elixir
  setup do
    oban_name = start_supervised_oban!(queues: [default: 3])
    Logger.put_module_level(MyBatchWorker, :all)

    on_exit(fn ->
      Logger.delete_module_level(MyBatchWorker)
    end)

    %{oban_name: oban_name}
  end
```

Then, in our `setup` function, we use the previous helper
`start_supervised_oban!/1` to start a new Oban supervisor that will only handle
jobs for the `default` queue. Then we say that we want to include all the
logging levels available in the `MyApp.MyBatchWorker` module, the `on_exit/1`
callback will clean up this setting at the end of our test. Finally, we put the
Oban supervisor's name in the test context.

```elixir
  test "testing our batch worker", %{oban_name: oban_name} do
    batch =
      ["foo@example.com", "bar@example.com"]
      |> Enum.map(&%{email: &1})
      |> MyBatchWorker.new_batch()

    assert capture_log(fn ->
             Oban.insert_all(oban_name, batch)
             Oban.drain_queue(oban_name, queue: :default)
           end) =~ "exhausted callback"
  end
```

And here is where we test our batch worker. First, we create a new batch with
`MyApp.MyBatchWorker.new_batch/1` and immediately insert with
`Oban.insert_all/2`. It is essential to pass the `oban_name` as the first
argument to that function. We drain the `default` queue right after using
`Oban.drain_queue/2`. The way I'm testing here that the `handle_exhausted/1` is
called is by capturing the log or side-effect produced for that callback, but
as I said before, you can do here whatever you want; you need to check the
side-effects produced by your callback.

But wait, something is missing in our previous `start_supervised_oban!/1`
helper. Do you remember that I mentioned before that the _plugins_ implement
the [GenServer](https://hexdocs.pm/elixir/GenServer.html) behaviour? Also, in
the `test` environment, we're using
[`Ecto.Adapters.SQL.Sandbox`](https://hexdocs.pm/ecto_sql/Ecto.Adapters.SQL.Sandbox.html#module-collaborating-processes)
to allow concurrent transactional tests. So we need to enable collaboration
from Oban processes to run these tests successfully for these conditions. All
these processes should use the same connection, so they all belong to the same
transaction.

From the `Ecto.Adapters.SQL.Sandbox` documentation, we have the following:

> The test above will fail with an error similar to:
>
> `** (DBConnection.OwnershipError) cannot find ownership process for #PID<0.35.0>`
>
> That's because the `setup` block is checking out the connection only for the test process. Once the worker attempts to perform a query, there is no connection assigned to it and it will fail.
>
> The sandbox module provides two ways of doing so, via allowances or by running in shared mode.

And also, we have:

> The idea behind allowances is that you can explicitly tell a process which checked out connection it should use, allowing multiple processes to collaborate over the same connection.

So, we need to include this allowance in our `start_supervised_oban!/1` helper.
Thankfully, Parker recently shared in the `#oban` channel the following gist to
[demonstrate how to integration test batch
callbacks](https://gist.github.com/sorentwo/1286196276d1adecbebfa082320a991b),
there you can see this clever piece to allow the Oban Producers, Plugins, and
other modules to use the checked out connection:

```elixir
{% raw %}
  # For Oban < v2.11 you can remove the `{:_, Oban.Peer}` part
  for key <- [{:_, Oban.Peer}, {:_, {:producer, :_}}, {:_, {:plugin, :_}}],
      pid <- Registry.select(Oban.Registry, [{{key, :"$2", :_}, [], [:"$2"]}]) do
    Ecto.Adapters.SQL.Sandbox.allow(MyApp.Repo, self(), pid)
  end
{% endraw %}
```

But again, this piece of code assumes you know some internals, so I will try to
explain a bit the previous code.

In this [excellent PR](https://github.com/sorentwo/oban/pull/331), Saša Jurić
introduced [Registry](https://hexdocs.pm/elixir/Registry.html) into Oban to
replace all the locally registered names, and also to hold the configuration
for those processes as part of the Registry metadata.

So, if you get everything that's in the Oban Registry you see something like:

```elixir
{% raw %}
iex(1)> Registry.select(Oban.Registry, [{{:"$1", :"$2", :"$3"}, [], [{{:"$1", :"$2", :"$3"}}]}])
[
  {{Oban, {:watchman, "default"}}, #PID<0.576.0>, nil},
  {{Oban, Oban.Notifier}, #PID<0.568.0>, nil},
  {{Oban, {:plugin, Oban.Plugins.Stager}}, #PID<0.571.0>, nil},
  {{Oban, {:foreman, "default"}}, #PID<0.574.0>, nil},
  {{Oban, {:plugin, Oban.Plugins.Pruner}}, #PID<0.570.0>, nil},
  {{Oban, {:supervisor, "default"}}, #PID<0.573.0>, nil},
  {{Oban, {:producer, "default"}}, #PID<0.575.0>, nil},
  {Oban, #PID<0.567.0>, %Oban.Config{...}},
  {{Oban, Oban.Midwife}, #PID<0.569.0>, nil}
]
{% endraw %}
```

Then, based on the previous `Registry` output, the `for` comprehension filters
the results based on the `key` pattern, and we return the `pid` (a.k.a.
`:"$2"`) for each of those coincidences. Once we get the `pid`, we explicitly
assign the test process's connection to each of these Oban processes.

And with the previous pattern, you can execute e2e tests, integration testing,
and so forth.

Finally, if you want to know more about how to test Oban jobs, I highly
recommend watching the talk [Testing Oban Jobs From the Inside Out][oban-talk]
from Parker Selbert.

## Conclusion

Oban offers some good helpers to run your Unit and Integration tests; you
should pay special attention to the [Oban.Testing][] documentation, and also
keep in mind that you also have helpers to test workers included in Oban Pro
such as [Batch][], [Chunk][], or [Workflow][].

But, indeed, there is always room for improvement. Especially when you want to
test your Oban configuration as soon as possible, it would be awesome to have
helpers validate all the settings, including plugins, at once. Also, in my
opinion, the plugins should have a uniform way to test their configuration. As
I mentioned in this [issue](https://github.com/sorentwo/oban/issues/632), a
possible solution could be:

> A public test helper to validate the Oban configuration, including plugins. I think it's also worth normalizing the _plugins_ under a contract or behavior; Oban now enforces defining an `init/1` function for plugins, but I think the behaviour should also include a `validate!/1` function. At least the `validate!/1` should help with the _testing experience_.
>
> There are also plugins in the Pro package that could include complex configurations, like the `Oban.Pro.Plugins.DynamicPruner`. So, I think it's worth it to offer a unified way to validate these configs.

I hope you find this article helpful, and you have a better idea of how to test
your Oban Workers and your Oban Configuration.

If you want to reach me, you can do it at [`@milmazz`][milmazz] on Twitter, or
you can find me in the `#oban` channel on the [Elixir Slack][slack].

That's all, folks! Thanks for reading.

## Acknowledgments

Thank you, [Parker Selbert][sorentwo], and [Andrew
Ek](https://andrewek.github.io) for reviewing drafts of this post.

[Batch]: https://hexdocs.pm/oban/batch.html
[Chunk]: https://hexdocs.pm/oban/chunk.html
[Ecto]: https://hexdocs.pm/ecto/Ecto.html
[Elixir]: https://elixir-lang.org
[ExUnit]: https://hexdocs.pm/ex_unit
[HexDocs]: https://hexdocs.pm/oban
[Oban.Testing]: https://hexdocs.pm/oban/Oban.Testing.html
[Oban]: https://getoban.pro
[Phoenix]: https://www.phoenixframework.org
[PostgreSQL]: https://www.postgresql.org
[Repeater]: https://hexdocs.pm/oban/Oban.Plugins.Repeater.html
[Workflow]: https://hexdocs.pm/oban/workflow.html
[async_stream]: https://hexdocs.pm/elixir/Task.html#async_stream/3
[complex-worker]: https://sorentwo.com/2021/05/06/composing-jobs-with-oban-pro.html
[license]: https://getoban.pro/pricing
[milmazz]: https://twitter.com/milmazz
[oban-talk]: https://www.youtube.com/watch?v=PZ48omi0NKU
[oban-web]: https://getoban.pro/oban
[partial-index]: https://www.postgresql.org/docs/current/indexes-partial.html
[regulator]: https://github.com/uwiger/jobs/blob/master/doc/erlang07g-wiger.pdf
[slack]: https://elixir-slackin.herokuapp.com/
[sorentwo]: https://sorentwo.com
[start_supervised]: https://hexdocs.pm/ex_unit/ExUnit.Callbacks.html#start_supervised!/2o
