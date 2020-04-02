---
title: Improve the codebase of an acquired product
author: milmazz
date: 2020-04-01T18:27:31Z
tags:
  - programming
  - elixir
slug: improve-the-codebase-of-an-acquired-product
---

In this article I'll share my experience improving the codebase of an acquired
product, this couldn't be possible without the help of a fantastic team. Before
diving into the initial diagnostic and strategies that we took to tackle
_technical debt_, I'll share some background around the acquisition. Let's
start.

<!--more-->

## Background

I wasn't involved in the acquisition process. I started working with this
client around a year after that decision; that's why in this article, I do not
mention those details. Maybe the only thing that I can share is that Elixir and
Phoenix power the product, and possibly at the time of the acquisition, fulfilled
its goal and covered some [opportunity costs][] for the client.

I'll assume that the previous developers made their best effort under a set of
constraints (time, team-size, among others) that I don't know how good or bad
they were. This codebase is now part of the team, and it's our team goal to
take ownership of this codebase, to accomplish that we establish some common
goals:

* Do not break current working features given that the system is in production,
  and it's serving millions of requests per day
* Add new features based on requirements or stories, the usual thing
* Improve the codebase along the way

After the acquisition process, the client knew that the codebase was a
"dumpster fire" (their words, not mine). Still, it was generating revenue, so
throwing away that product was out of the question. Thankfully, the client
allowed some time to improve the codebase.

At the same time, we delivered new features, even accepted the fact that we had
to delay some deployments until we thought it was the right time to do it.
Again, I mention this because this kind of agreement is difficult to get
sometimes.

## Diagnostics

The repository followed a poncho structure; the state of some of the
applications were the following:

* Zero (none, nada) unit tests, this was *awful* because it made the whole
refactoring process more painful and slow.
* No documentation in the modules, public functions, only a brief `README` file
  describing the goal of the application, and that was all.
* A lot of 3rd party dependencies, the most painful part here was the
  dependency of different _storage services_. For example, Airtable, Mongo,
  PostgreSQL.
* The goals of some project dependencies were the same, like HTTP clients, JSON
  parsers, job queue processors, among others. Even a team-partner said
  something like: "...background worker libraries are like Pokemon, and I think
  we have collected them all", so that could give you an idea of our situation.
* We found large portions of duplicate implementations, for example, HTTP
  clients implementations embedded in some of the poncho applications instead
  of having that HTTP client implementation as a dependency.

## Strategies

The team decided to tackle these improvements in a series of continuous steps.

* Move the other "storage services" into PostgreSQL one table at a time. The
  current team has a better knowledge of this ORDBMS, and it's been offering
  excellent results.
* Delete unused code; this was a low-hanging-fruit for us using some tools that
  I'll mention later in this article.
* Separate the implementation of 3rd party HTTP clients from the main
  components. So, reaching a given 3rd party service must be done via a
  separate application.
* Improve some design decisions
* Introduce error monitoring, that allowed us to discover, triage, and
  prioritize errors in real-time.

## Tooling support

As I mentioned before, removing unreachable or unused code was easy for us, and
with that, we removed a considerable burden when trying to understand how the
applications work. Here the critical player in my workflow was [unused][],
which is a command-line tool, written in Haskell, that helps you to identify
unused code in multiple languages and that includes Elixir.

Before we proceeded with the code removal, we were confident that the code we
were removing wouldn't break the program, one way we used to gain more
confidence was `mix xref callers CALLEE`, which prints all the callers of the
given `CALLEE`[^1].

Then, we started adding unit tests to each project. Unit testing is a
first-class concern on our team, so, before proceeding with a _storage service
migration_ (e.g., Moving a Mongo collection into a PostgreSQL table), we added
unit tests, using the [ex_unit][] framework, around the modules and functions
that we were planning to migrate. While doing this, we did some test coverage
analysis with `mix test --cover` to identify untested areas of the code.

Adding unit tests around the areas we were planning to migrate increased our
confidence in the following steps, and also helped us to discover other bugs
along the way. So, that was a win-win situation for us.

Another great addition was the _error monitoring_ process, before doing that we
were almost blind about the things that were failing in production, well,
that's not entirely accurate, we had access to the logs, but nobody in the team
was checking those constantly.

The team was already using Sentry in other applications, so we decided to use
Sentry here too. We started linking Sentry with our GitHub repository. We proceeded to tackle one bug at a time in multiple applications, giving priority
to the events with a higher number of occurrences. To provide you with an idea of the
situation at the beginning, some of the applications had so many errors that
Sentry responded with HTTP Errors 429 (Too Many Requests), that's why some
team-members coined the term "dumpster fire" to this legacy application. Today
the situation is another story, so I recommend adding an _error monitoring_
tool as soon as possible in your projects, and this includes your staging
environments too.

We used other tools too, like `dialyzer`, it's true, `dialyzer` can be
intricate sometimes. Still, I would say that it could be beneficial to discover
bugs in your code and also incorrect types and specifications (a.k.a.
[typespecs][typespecs]).

Last but not least, [credo][] has been useful to run static code analysis in
our codebase and to promote some consistency and readability.

While we haven't included [ex_doc][] in our workflow yet, we have added a bunch
of documentation in our code, and it's already paying dividends; newer team
members mentioned that the onboarding process is more accessible thanks to that
documentation.

## Design

While we were refactoring, we made many decisions to improve the current
codebase, among them, one of the most recent was that we identified that the
company was losing money when an agent/user started a session (LOGON) in a 3rd
party system. Still, the agent wasn't READY to operate. The interaction with
the 3rd party service was something like this:

* Every second a process collected data from the 3rd party service because they
  didn't offer a webhook that allowed us to subscribe to those events
* Then, the producer sent all the collected messages to some other modules via
  Phoenix Channels, among the one that was in charge of forcing those agents to
  be READY to operate
* The process that put READY those agents was only one instance of
  a GenServer

So, given the previous scenario and given that a GenServer can process only one
message at a time, we had some agents in the NOT_READY state waiting for the
FORCE_READY operation. What we did, in this case, was to split the input per
agent IDs and then dynamically create GenServers where its state was all around
the agent/user, once the agent finished its session, we stopped that GenServer
instance. This change allowed us to increase our FORCE_READY operations thanks
to the concurrency and prepared us for a new feature, which kicked users after
a configurable time of inactivity.

Previously the kicking process was done by our support staff manually. We added
the "kicking inactive users" feature while we were doing a work re-retreat (the
company is 100% remote). You can't imagine the happiness shown by our support
staff after deploying that feature and seeing how the application was
automatically kicking inactive users. Sometimes, as a developer, we lose the
perspective and impact that we can make on people's lives; it was a humbling
experience.

## Rewards

* Staff & support teams are enjoying the fruits of these improvements while
  doing less manual processes. Of course, we still have some areas that we need
  to polish. We're getting there
* Improved the onboarding experience for newer developers, while also improving
  the experience for older developers
* Our confidence while doing changes have increased
* Deliver new features more quickly
* Savings, while we haven't completed the whole migration from some storage
  services, our estimate is to save the company more than 20K a month after
  finishing the whole process, which, according to our plan, is at the end of
  the next month.

## Wrapping Up

Elixir and Phoenix, even in the worst-case scenarios, have demonstrated that it
allows some companies to deliver excellent results. But, it's the team culture,
and following effective practices, that enables to improve the user experience,
while enhancing the performance and the quality of the code.

[^1]: Another tool that you should probably check out is [`Mix Unused`][mix_unused]. I haven't used it yet, because I recently knew about its existence, but I plan to do it sooner than later.
[opportunity costs]: https://www.investopedia.com/terms/o/opportunitycost.asp
[unused]: https://github.com/joshuaclayton/unused
[mix_unused]: https://github.com/hauleth/mix_unused
[typespecs]: https://hexdocs.pm/elixir/typespecs.html
[credo]: https://github.com/rrrene/credo
[ex_doc]: https://github.com/elixir-lang/ex_doc
[ex_unit]: https://hexdocs.pm/ex_unit/ExUnit.html
