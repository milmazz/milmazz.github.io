---
title: "Oban: job processing library for Elixir"
author: milmazz
date: 2022-02-11T18:27:31Z
tags:
  - programming
  - elixir
---

After working for years on different organizations, one common theme
is scheduling background jobs. In this article, I'll share my experience with [Oban][], an open-source job processing
package for [Elixir][]. I'll also cover some features, like real-time monitoring
with Oban Web and complex workflow management with Oban Pro.

<!--more-->

This article is the first of a series; later we will explore other important topics such as:

* Oban: Testing your Workers and Configuration
* Oban in production

Stay tuned!

Let's begin with an overview of Oban.

## Oban Overview

In this context, when we talk about Oban, I'm not referring to the [town in Scotland](https://en.wikipedia.org/wiki/Oban). Instead, I will be talking about an Elixir library that offers a
background job system built on top of [PostgreSQL][] with the primary goals of
reliability, consistency, and observability. One of the cool features of Oban,
given that it's built on top of PostgreSQL is that you can enqueue jobs and other
database changes, ensuring that everything is committed or rolled back
atomically.

I will also talk about _Oban Pro_, which, according to the [official site][Oban]:

> Oban Pro is a collection of plugins, workers and extensions that improve Oban's reliability and make difficult workflows possible.

You can see a comparison between the open-source and the paid version [here](https://getoban.pro/#feature-comparison).

Let's start by examining the following configuration:

```elixir
config :my_app, Oban,
  repo: MyApp.Repo,
  queues: [default: 5, uno: 3],
  engine: Oban.Pro.Queue.SmartEngine,
  plugins: ...
```

The `repo` option specifies the [Ecto][] repository used to insert and retrieve jobs. The `queues` option is always, except when you use `false` as a value, a keyword list where the keys are the queue names, and its value specifies the concurrency limit. For our specific configuration, we define two queues, `default` with a local concurrency limit of 5 and the `uno` queue, which I will use later to explain how to schedule _one-off_ jobs, the local concurrency limit for this queue is 3.

We can extend Oban functionality via plugins and callback modules as I already mentioned. Oban Pro takes advantage of this feature providing a collection of plugins, workers, and extensions.

One extension offered by Oban Pro is the
`Oban.Pro.Queue.SmartEngine`. It is an alternate queue engine that enables true global concurrency and global rate limiting. The open-source package offers a limited basic engine, `Oban.Queue.BasicEngine`.

The following list of plugins is for an _advanced_ configuration, and it uses plugins from both Oban and Oban Pro. Please adjust the following based on your scenarios:

```elixir
  plugins: [
    Oban.Plugins.Gossip,
    Oban.Plugins.Stager,
    Oban.Pro.Plugins.BatchManager,
    Oban.Pro.Plugins.Lifeline,
    Oban.Pro.Plugins.Reprioritizer,
    {
      Oban.Pro.Plugins.DynamicPruner,
      mode: {:max_age, {1, :day}},
      limit: 25_000,
      queue_overrides: [
        uno: {:max_age, :infinity}
      ],
      state_overrides: [
        cancelled: {:max_age, {5, :days}},
        discarded: {:max_age, {5, :days}}
      ]
    },
    {
      Oban.Pro.Plugins.DynamicCron,
      crontab: [
        {"30 7 * * *", MyApp.MyWorker},
        {"@reboot", MyApp.Migrations.MyDataMigrationWorker}
      ]
    }
  ]
```

You can see that we're using some plugins, like `Oban.Pro.Plugins.Lifeline`,
which offers a way to rescue orphaned jobs. Or the
`Oban.Pro.Plugins.Reprioritizer`, which prevents queue starvation by
automatically adjusting priorities to ensure all jobs are eventually processed; this plugin is handy when you're using different priorities in your `Oban.Worker`, a classic example is given in the plugin documentation:

> For example, a queue that processes jobs from various customers may
> prioritize customers that are in a higher tier or plan. All high priority (`0`)
> jobs are guaranteed to run before any with lower priority (`1..3`), which is
> wonderful for the higher tier customers but can lead to resource starvation.
> When there is a constant flow of high priority jobs the lower priority jobs
> will never get the chance to run.
>
> The `Reprioritizer` plugin automatically adjusts lower job's priorities so that
> all jobs are eventually processed.

In another section of this article, I will be giving more details about the
`Oban.Pro.Plugins.DynamicPruner` plugin, so let's skip that for now.

At the end of our plugin list, you can see that we're using
`Oban.Pro.Plugins.DynamicCron`, which is an advanced version of the
`Oban.Plugins.Cron` plugin for cron scheduling. The pro version allows changing
its configuration globally across your entire cluster at runtime.

For more details about Oban, visit the official site at [getoban.pro][Oban] or
on HexDocs at [hexdocs.pm/oban][HexDocs]

Now let's review some of the features from Oban Web.

## Oban Web

The best place to check what Oban Web has to offer is the [live Web dashboard
demo][oban-web], according to the authors:

> [The Oban Web Dashboard Demo is] a playful combination of randomly generated
> workers using fake data and random failures makes the demo a chaotic
> simulation of a production workload.
> ...
> The demo is a beautiful canary because it uses the latest OSS, Web, and Pro
> releases, utilizing all the plugins and most available features. With error
> monitoring, we receive notifications that help us diagnose and fix issues
> from a constantly running production instance, often (but not _always_) before
> any customers report a problem! It's crowdsourcing _and_ dogfooding rolled into
> one,

Let's start reviewing a few parts from this live demo.

### Dashboard

![oban-dashboard](/images/2022-02-11-oban-job-processing-library-for-elixir/oban-dashboard.png)

In this image, you can see the list of jobs that Oban is executing. On the
sidebar, you see different sections, such as nodes, states, and queues.

Currently, they have two nodes acting as workers to run Oban Jobs in this demo.

Each node has six queues, one of them is `analysis`, and for this queue on each node, we have a local limit of 20, giving us a total of 40, which is what you
see in the `limit` column. You can see that the `mailers` and the `media`
queues have some symbols in the `mode` column. The _chart line down_ character in
the queues `media` and `mailers` means that those queues are rate limited,
which is possible given that this demo uses the `SmartEngine`. Still, you can see another icon for the `media` queue, the _globe_ icon, meaning that the `media` queue has a global limit in the cluster.  

You can also see that the demo has already completed more than 359k jobs, and
there is just one job available. One thing to notice here is that regardless of
the number of jobs available, Oban will not process more jobs than is allowed
on each queue at a given time, imposing a back-pressure mechanism, which is essential to avoid overloading our system.

### Job details

![oban-job-executing](/images/2022-02-11-oban-job-processing-library-for-elixir/oban-job-executing.png)

In this image, you can see the job details in real-time; for example, in this
capture, you can see the current state, the specific arguments for this job,
which node is executing this job, schedule time, and so on. Note that you have a button on the upper right side that could allow you to cancel the
job that's being executed.

![oban-job-completed](/images/2022-02-11-oban-job-processing-library-for-elixir/oban-job-completed.png)

In this image, you can see that a specific job is completed without errors.

![oban-job-discarded](/images/2022-02-11-oban-job-processing-library-for-elixir/oban-job-discarded.png)

But in this image, you can appreciate that this job was discarded, meaning that
we reached the maximum number of attempts, but on each try, we got errors,
this traceback view increases the observability and could help you find an issue on your code, or, if you're dealing with a flaky third-party service, you can hit the _Retry_ button, for example.

### Queues

![oban-queues](/images/2022-02-11-oban-job-processing-library-for-elixir/oban-queues.png)

If you press the _Queues_ tab, you can see more details per queue, including
nodes, how many jobs per queue are available, their local and global limit, if
they are rate-limited, and so on. If you have the proper permissions, you can even stop or resume each queue on-demand from here. For example, in the previous screenshot, you
can see that I stopped the `analysis` queue in one of the available nodes.

### Smart Engine extension

![oban-smart-engine](/images/2022-02-11-oban-job-processing-library-for-elixir/oban-smart-engine.png)

Under the _Queues_ tab, you will see an image
similar to the previous one if you click on a queue name. Using the `SmartEngine`, you can set limits by rate, global, or both per queue.

One neat feature of the rate limit section is that you can go a bit further
and apply _partitioned rate-limiting_ by `worker`, `args`, or both within a queue.
For example, in the `media` queue, you can see that there is a _local limit_ of
10, a _global limit_ of 25, but only 20 jobs are allowed per worker (_Partition
Field_), every 60 seconds, across every instance of the `media` queue in this
cluster.

Here you can see that the `mailers` queue is rate limited. We can
define Oban Jobs or Workers that interact with external services, and we just
set the rate limit at the queue level. There is no need to worry about rate limit implementation at the _worker level_.

Okay, I covered enough Oban Pro and Oban Web features with this section. But, if you want to know more details about the Oban Architecture, I recommend checking these slides [The Architecture of Oban](https://speakerdeck.com/sorentwo/the-architecture-of-oban) from
Parker Selbert.

Now, let's review some conventions that I tend to follow.

## Conventions

I will briefly examine some conventions I like to follow when using Oban in the following sub-sections.

### Naming and file/directory organization

I tend to follow this code organization for Oban workers; adding subdirectories under the `workers` directory is valid.

```console
my_app/
├── README.md
├── lib
│   ├── my_app
│   │   └── workers
│   │       └── archive_account.ex
│   └── my_app.ex
├── mix.exs
└── test
    ├── my_app
    │   └── workers
    │       └── archive_account_test.ex
    ├── my_app_test.exs
    └── test_helper.exs
```

And the module naming is as follows:

* `MyApp.Workers.ArchiveAccount` for the worker implementation
* `MyApp.Workers.ArchiveAccountTest` for the unit tests associated with the previous worker implementation.

I've worked on some projects that follow the [Phoenix][] style, meaning that
the directory structure is very similar to the previous one, but the file names are a bit different:

```console
my_app/
├── README.md
├── lib
│   ├── my_app
│   │   └── workers
│   │       └── archive_account_worker.ex
│   └── my_app.ex
├── mix.exs
└── test
    ├── my_app
    │   └── workers
    │       └── archive_account_worker_test.ex
    ├── my_app_test.exs
    └── test_helper.exs
```

Also, the module names are a bit different:

* `MyApp.ArchiveAccountWorker` for the worker implementation
* `MyApp.ArchiveAccountWorkerTest` for the unit tests associated with the previous worker implementation.

I've worked fine with both approaches; once your team has made the decision, you should stick with it; that's the most important thing for me to
be honest.

So, before starting coding, take some time and define what code
organization and naming convention you and your team want to follow.

I will follow the "Phoenix" way of doing things in the following code samples.

### Keep calls to `Oban.insert` or `Oban.insert_all` contained in your worker

I highly recommend keeping the knowledge about how to enqueue an Oban job in
their `Oban.Worker` implementation. Following this approach, you also avoid
polluting your controllers, resolvers, or contexts with a sequence of calls
like the following:

```elixir
my_job_args
|> MyApp.MyWorker.new()
|> Oban.insert()
```

Instead, you can create a `enqueue/1` function like this:

```elixir
defmodule MyApp.MyWorker do
  use Oban.Worker,
    queue: :things,
    max_attempts: 5,
    unique: [period: _period_in_seconds = round(:timer.hours(1) / 1000)]

  alias MyApp.Thing

  @doc """
  Enqueues an Oban job to do something with the given thing
  """
  @spec enqueue(Thing.t()) :: {:ok, Job.t()} | {:error, Job.changeset()} | {:error, term()}
  def enqueue(%Thing{id: thing_id}) do
    %{thing_id: thing_id}
    |> new()
    |> Oban.insert()
  end

  @impl Oban.Worker
  def perform(%Job{args: %{"thing_id" => _thing_id}} = _job) do
    :ok
  end
end
```

Remember that your `enqueue` function doesn't need to have an arity of one;
adjust the number of arguments depending on what your worker expects.

Even for more _complex workers_, you can apply the same convention, for example:

```elixir
defmodule MyApp.TranscodeWorker do
  use Oban.Pro.Workers.Workflow

  alias MyApp.IndexingWorker
  alias MyApp.NotifyWorker
  alias MyApp.RecognizeWorker
  alias MyApp.SentimentWorker
  alias MyApp.TopicsWorker
  alias MyApp.TranscribeWorker

  def process_video(video_id) do
    args = %{id: video_id}

    new_workflow()
    |> add(:transcode, new(args))
    |> add(:transcribe, TranscribeWorker.new(args), deps: [:transcode])
    |> add(:indexing, IndexingWorker.new(args), deps: [:transcode])
    |> add(:recognize, RecognizeWorker.new(args), deps: [:transcode])
    |> add(:sentiment, SentimentWorker.new(args), deps: [:transcribe])
    |> add(:topics, TopicsWorker.new(args), deps: [:transcribe])
    |> add(:notify, NotifyWorker.new(args), deps: [:indexing, :recognize, :sentiment])
    |> Oban.insert_all()
  end

  # ...
end
```

*NOTE: The previous example was borrowed and slightly modified from the _Workflow
Example_ available in the [Composing Jobs With Oban Pro][complex-worker] post by [Shannon and Parker][sorentwo].*

## One-off jobs

The easiest way to run one-off functions is via `bin/RELEASE_NAME remote` (or `remote_console` if you use `distillery` to create your release) on
production nodes, but that's not always available. In these cases, you can use Oban to run
your one-off jobs.

If you plan to do a _data migration_, for example, consider wrapping this
process into an [Oban][] job doing the following.

Add a new file under `lib/my_app/workers/migrations/`, leaving the suffix
`_worker.ex`, for example:
`lib/my_app/workers/migrations/my_data_migration_worker.ex`, your new
worker module should be something similar to the following:

```elixir
defmodule MyApp.Migrations.MyDataMigrationWorker do
  use Oban.Worker,
    queue: :uno,
    max_attempts: 5,
    unique: [period: :infinity, states: Oban.Job.states()]

  @impl Oban.Worker
  def perform(%Job{} = job) do
    # TODO: data migration
    # should return a valid value
    # See: https://hexdocs.pm/oban/Oban.Worker.html#module-defining-workers
  end
end
```

From the previous code snippet, notice that you must use the `uno` queue and
define a `unique: [period: :infinity, states: Oban.Job.states()]` to indicate that any attempt to enqueue a subsequent job will be considered a duplicate _as long as jobs are retained_
in the database, and for the specific case of the `uno` queue, we keep those jobs indefinitely. You can adjust the number of `max_attempts` based on your
scenario.

You can test your data migration adding a new file under
`test/my_app/workers/migrations/my_data_migration_worker_test.exs`.

Once you have unit tested your worker following the suggestions that I will offer in the
_Testing your Workers and Configuration_ article, proceed to add an entry in your configuration file:

```elixir
config :my_app, Oban,
  # ..
  plugins: [
    # ...
    {
      Oban.Pro.Plugins.DynamicCron,
      crontab: [
        # ...
        {"@reboot", MyApp.Migrations.MyDataMigrationWorker},
        # ...
      ]
    }
  ]
```

The `@reboot` string is a "non-standard syntax" that allows executing the given
job at boot time in one single node in the cluster.

## Challenges

Now, I think it's time to examine a few challenges that you could find while working with Oban.

### Inserting Oban jobs in bulk

Sometimes you need to enqueue many Oban Jobs at once or in bulk.

Please, don't do this:

```elixir
my_data_stream
|> Stream.map(&MyApp.MyWorker.new(%{asset_id: &1.id}) # <- use the arguments you really need for your worker
|> Enum.map(&Oban.insert/1)
```

This will produce a lot of roundtrips to the database, instead, you should use `Oban.insert_all/4`:

```elixir
my_data_stream
|> Stream.map(&MyApp.MyWorker.new(%{asset_id: &1.id})
|> Enum.map(&Oban.insert_all/1)
```

While the previous approach avoids doing many roundtrips to the database, you can have problems depending on the number of jobs you're trying to insert
at once. Keep in mind that PostgreSQL's binary protocol has a limit of 65,535
parameters that you may send in a single call. That presents an upper limit on
the number of rows you may insert at one time and, therefore, the number of
jobs you may insert in all at once.

So, it's safer to split the previous stream into chunks:

```elixir
timeouts = 
  my_data_stream
  |> Stream.map(&MyApp.MyWorker.new(%{asset_id: &1.id})
  |> Stream.chunk_every(chunk_size)
  |> Task.async_stream(&Oban.insert_all/1, ordered: false, timeout: timeout_ms, on_timeout: :kill_task)
  |> Stream.filter(& &1 == {:exit, :timeout})
  |> Enum.count()

# TODO: handle timeouts 
```

Here we're using our _beloved_ [Task.async_stream/3][async_stream], that will return a stream that runs the given function, `Oban.insert_all/1`, _concurrently_ over each _chunk_ in the enumerable.

Adjust the `chunk_size` accordingly to handle your specific scenarios.

One important thing to keep in mind when you use `Oban.insert_all/2,4` is that
you can insert duplicate jobs, even if your worker defines `unique` options
like:

```elixir
defmodule MyApp.MyWorker do
  use Oban.Worker,
    unique: [period: _period_in_seconds = round(:timer.hours(1) / 1000)]

  # ...
end
```

As noted in the documentation:

> [Oban.insert_all/2] insertion respects `prefix` and `log` settings, but it _does not use_ per-job unique configuration. You must use `insert/2,4` or `insert!/2` for per-job unique support.

But, sometimes, using `Oban.insert/2,4` is too costly. You might want to insert
hundreds or thousands of unique jobs as fast as possible; in these cases, you
have two possibilities, at least that I'm aware of so far.

The first one is to guarantee the uniqueness in the stream pipeline, but, this
could be risky because you are discarding the possibility of introducing a duplicate job that's already in the `oban_jobs` table. In these cases, it's
safer to set up a [partial unique index][partial-index] in PostgreSQL, you can
set up a migration to create your _partial unique index_ as follows:

```elixir
defmodule MyApp.Repo.Migrations.CreateUniqueIndexForMyWorker do
   use Ecto.Migration

   @disable_ddl_transaction true
   @disable_migration_lock true

   @index_name "oban_jobs_unique_my_worker_index"
   @worker_name "MyApp.MyWorker"

   def up do
     execute("""
     CREATE UNIQUE INDEX CONCURRENTLY #{@index_name} ON oban_jobs (worker, args) WHERE worker = '#{@worker_name}'
     """)
   end

   def down do
     execute("DROP INDEX IF EXISTS #{@index_name}")
   end
 end
```

You can include more conditions to your _partial unique index_, adjust this
settings based on your specific case.

But you may be wondering why this would work at all. It happens that
`Oban.insert_all/2,4` it's a wrapper around `Repo.insert_all/4` and it sets the
`on_conflict` option to `:nothing`. Better, you can test this behavior, once you
have run the previous migration yourself by creating s unit test similar to
this one:

```elixir
refute payload
       |> MyBatchWorker.new_batch(batch_id: project.id)
       |> Oban.insert_all()
       |> Enum.any?(& &1.conflict?)

# second time all the jobs must create a conflict, but not an insertion
assert payload
       |> MyBatchWorker.new_batch(batch_id: project.id)
       |> Oban.insert_all()
       |> Enum.all?(& &1.conflict?)
```

In this unit test, we use the key `:conflict?` to detect the job uniqueness. From the Oban README, we have:

> When unique settings match an existing job, the return value of `Oban.insert/2`
is still `{:ok, job}`. However, you can detect a unique conflict by checking the
jobs' `:conflict?` field. If there was an existing job, the field is `true`;
otherwise it is `false`.

So, in the first pass, we check that the Oban Jobs doesn't have _any_ entry like: `%Oban.Job{conflict?: true}`. We check that _all_ the entries have `%Oban.Job{conflict?: true}` in the second pass.

Nice, huh?

## Complex workers

I won't explain in detail the Workers offered by `Oban.Pro`, not because I
think they don't deserve a space here, but because [Shannon and Parker][sorentwo]
already did a fantastic job in their post [Composing Jobs With Oban
Pro][complex-worker], they will take you on a tour of the workers included in
Pro and explore some real-world use-cases where each one shines.

## Limitations

If you have reached this point, you undoubtedly noticed that I have _enjoyed_ working with Oban so far, and at this point, I could be _biased_, so I want to take a step back to add some balance and mention some of Oban's limitations.

* Oban is "job processing in Elixir, backed by modern PostgreSQL". I don't see this as a limitation, but you can't use Oban if you don't use PostgreSQL in your stack. Thankfully that's not my case for many years :)
* If you use PostgreSQL, but you have an overloaded database currently, I don't recommend adding more pressure with Oban. You have a bigger problem that you need to solve as soon as possible—you need to figure out what's overloading your database and fix it!
* If you still want to use Oban, but you're thinking of adding a new PostgreSQL database just for the Oban workers, keep in mind that you will lose one of the most relevant features from Oban, which is the built-in transactional control.
* If you want good performance in Oban Web, you should keep your `oban_jobs` table lean. I've noted some delays in the UI at around ten million records.
* If you're trying to ingest high throughput event streams, Oban probably isn't the solution you need. You can process more than 15K jobs per second with Oban on a single node; of course, that number will highly depend on what your worker does. But you should increase that throughput significantly if you use the _Batch Worker_ behaviour. Still, sometimes that's not enough, and you should use a _message broker_, like [rabbitmq](https://github.com/rabbitmq) in conjunction with [Broadway](https://elixir-broadway.org) or [GenStage](https://github.com/elixir-lang/gen_stage). Still, the initial learning curve of the latter tools could be higher, and you also need to make other considerations, like considering the state of your processes while you deploy.

## Wishlist

In the same vein as the previous section, there are a few things that I would like to see in Oban:

* Autoload regulation, suppose that your deployment in production shares the API server with your Oban Workers; these processes will compete with each other for resources. With a regulation framework built-in, your queues could be constrained a bit more if the load in your nodes is too high. If you don't have a rate limit in your queue, you could automatically increase the _concurrency limit_ if the load in the node is low. An excellent reference to this topic can be found in the paper [Generic Load Regulation Framework for Erlang][regulator] by Ulf Wiger. As far as I can tell, this feature is already in the _roadmap_.
* A callback in the `{Fixed,Dynamic}Pruner` plugins; that way, before proceeding with the deletion, you can store or transfer those records into cold storage or somewhere else. I recently [opened an issue](https://github.com/sorentwo/oban/issues/633) about this feature.

Do you have things that you would like to see in Oban? If that's the case, and you want to share those wishes with me, you can reach me at [`@milmazz`][milmazz] on Twitter, or you can find me in the `#oban` channel on the [Elixir Slack][slack].

## Community

You can connect with the Oban authors, contributors, and other Oban users through any of these channels:

* `#oban` channel on the [Elixir Slack][slack]. Here you can see new announcements about new Oban releases.
* Follow `@sorentwo` on Twitter for tips, announcements, and news about Oban
* If you want to contribute to the OSS project, feel free to do it at <https://github.com/sorentwo/oban>. You might start with the [issues list](https://github.com/sorentwo/oban/issues). I've been able to contribute more than a dozen times, but on each occasion that I had a doubt, the authors were welcoming and offered good references, which is my general experience in the Elixir community, so don't be shy and join the team :)

## Conclusion

Oban is a solid solution to handle your background jobs in Elixir. It keeps true to what they offer in the README, like fewer dependencies, isolated queues, transactional control, unique, scheduled, and recurrent jobs, telemetry integration, and much more.

After all this, I think it's clear that if you or your company need a
_background job system_ in Elixir, I recommend buying the [Oban Web+Pro
license][license], keep in mind that when you buy the license, you're helping
[Shannon and Parker][sorentwo], to keep investing their time building more
features for the open-source release of [Oban][].

If you have any pattern that you follow when you use Oban and you want to share that with me, you can reach me at [`@milmazz`][milmazz] on Twitter, or you can find me in the `#oban` channel on the [Elixir Slack][slack].

That’s all folks! Thanks for reading.

## Acknowledgments

Thank you Parker Selbert for reviewing drafts of this post.

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
[start_supervised]: https://hexdocs.pm/ex_unit/ExUnit.Callbacks.html#start_supervised!/2
