---
redirect_from: "/archivos/2014/03/10/libturpial-needs-your-help/"
author: milmazz
comments: true
share: true
date: 2014-03-10 21:00:00
layout: post
slug: libturpial-needs-your-help
title: 'libturpial needs your help'
tags:
- python
---

Do you want to begin to contribute into [libturpial][] codebase but you don't
know where to start?, that's ok, it also happens to me sometimes, but today, the
reality is that we need your help to fix some errors reported by our style
checker ([flake8][]), this errors are:

* `E126`: continuation line over-indented for hanging indent
* `E128`: continuation line under-indented for visual indent
* `E231`: missing whitespace after `:`
* `E251`: unexpected spaces around keyword / parameter equals
* `E261`: at least two spaces before inline comment
* `E301`: expected 1 blank line
* `E302`: expected 2 blank lines
* `E303`: too many blank lines
* `E501`: line too long
* `E711`: comparison to `None` should be `if cond is not None:`
* `E712`: comparison to `False` should be `if cond is False:` or `if not cond:`
* `F401`: imported but unused
* `F403`: unable to detect undefined names
* `F821`: undefined name
* `F841`: local variable is assigned to but never used
* `F999`: syntax error in doctest
* `W291`: trailing whitespace
* `W391`: blank line at end of file
* `W601`: `.has_key()` is deprecated, use `in`

As you can see, some errors are really easy to fix but are too many for us, so
please, we desperately need your help, help us!

The following is how we expect the community can contribute code into
`libturpial` via [Pull Requests][pull-requests].

1) If you don't have a [Github][] account, please create one first.

2) Fork our project

3) Install [Git][], after that you should setup your name and email:

    {% highlight bash %}
    $ git config --global user.name "Your Name, not your Github nickname"
    $ git config --global user.email "you@example.com"
    {% endhighlight %}

4) Set your local repository

    {% highlight bash %}
    $ git clone https://github.com/your_github_nickname/libturpial.git
    {% endhighlight %}

5) Set `satanas/libturpial` as your `upstream` remote, this basically tells Git
that your are referencing libturpial's repository as your main source.

    {% highlight bash %}
    $ git remote add upstream https://github.com/satanas/libturpial.git
    {% endhighlight %}

5.1) Update your local copy

    {% highlight bash %}
    $ git fetch upstream
    {% endhighlight %}

6) Verify that no one else has been working on the same bug, you can check that
in our [issues][] list, in this list you can also check
[Pull Requests][pull-requests] pending for the [BDFL][] approval.

7) Working on a bug

7.1) In the first place, install [tox][]

    {% highlight bash %}
    $ pip install tox
    {% endhighlight %}

7.2) Then, create a branch that identifies the bug that you will begin to work
on:

    {% highlight bash %}
    $ git checkout -b E231 upstream/development
    {% endhighlight %}

In this example we are working on the bugs of the type: **E231** (as indicated
by our style checker, `flake8`)

7.3) Make some local changes and commit, repeat.

7.3.1) Delete the error code that you will begin to work from the `ignore` list
located at the `flake8` section in the `tox.ini` file (located at the root of
the project)

Example:


    {% highlight ini %}
    # Original list:
    ignore = E126,E128,E231,E251,E261,E301

    # After we decide to work in the E231 error:
    ignore = E126,E128,E251,E261,E301
    {% endhighlight %}

7.3.2) Execute `tox -e py27` to check the current errors.

7.3.3) Fix, fix, fix...

7.3.4) Commit your changes

    {% highlight bash %}
    $ git commit -m "Fixed errors 'E231' according to flake8."
    {% endhighlight %}

7.4) In the case that you fixed all errors of the same type, please delete the
corresponding line in the `tox.ini` file (located at the root of the project)

7.4.1) Don't forget to commit that.

8) Publish your work

    {% highlight bash %}
    $ git push origin E231
    {% endhighlight %}

Please, adjust the name `E231` to something more appropiate in your case.

9) Create a Pull Request and don't hesitate to bug the main maintainer until
your changes get merged and published. Last but not least, don't forget to add
yourself as an author in the AUTHORS file, located at the root of the project.

10) Enjoy your work! :-)

If you want to help us more than you did already you can check our [issues][]
list, also, you can check out the [Turpial][] project, our light, fast and
beautiful microblogging client written in Python, currently supports
[Twitter][], and [identi.ca][].

You can find more details on our [guide][], also, in case
of doubt don't hesitate to reach us.

## About libturpial

*libturpial* is a Python library that handles multiple microblogging protocols.
It  implements a lot of features and aims to support all the features for each
protocol. At the moment it supports [Twitter][] and [Identi.ca][] and is the
backend  used for *Turpial*.

## About Turpial

Turpial is an alternative client for microblogging with multiple interfaces. At
the moment it supports Twitter and Identi.ca and works with Gtk and Qt
interfaces.

[libturpial]: https://github.com/satanas/libturpial
[Turpial]: https://github.com/satanas/Turpial
[flake8]: https://pypi.python.org/pypi/flake8‎
[Github]: https://github.com
[Git]: http://git-scm.com/download
[issues]: https://github.com/satanas/libturpial/issues
[guide]: http://libturpial.readthedocs.org/en/latest/
[Twitter]: https://twitter.com
[identi.ca]: https://identi.ca
[pull-requests]: https://help.github.com/articles/using-pull-requests
[BDFL]: http://en.wikipedia.org/wiki/Benevolent_Dictator_for_Life‎
[tox]: https://pypi.python.org/pypi/tox‎
