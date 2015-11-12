---
redirect_from: "/archivos/2014/06/24/the-dry-principle/"
author: milmazz
comments: true
share: true
date: 2014-06-24T19:00:00-05:00
layout: post
slug: dont-repeat-yourself-principle
title: 'The DRY principle'
tags:
- jquery
- javascript
- programming
---

The [DRY][] (*Don't Repeat Yourself*) principle it basically consist in the
following:

> Every piece of knowledge must have a single, unambiguous, authoritative representation within a system.

That said, it's almost clear that the DRY principle is against the code
duplication, something that in the long-term affect the maintenance phase, it
doesn't facilitate the improvement or code refactoring and, in some
cases, it can generate some contradictions, among other problems.

Recently I have inherited a project, and one of the things that I noticed in
some part of the source code are the following:

{% gist milmazz/ed5d4aab90ea70deda9c original.js %}

After a glance, you can save some bytes and apply the *facade* and *module*
pattern without breaking the API compatibility in this way:

{% gist milmazz/ed5d4aab90ea70deda9c first_iteration.js %}

But, if you have read the [jQuery][] documentation it's obvious that the
previous code portions are against the DRY principle, basically, this functions
expose some shorthands for the `$.ajax` method from the jQuery library. That
said, it's important to clarify that [jQuery][], from version 1.0, offers some
shorthands, they are: `$.get()`, `$.getJSON()`, `$.post()`, among others.

So, in this particular case, I prefer to break the backward compatibility and
delete the previous code portions. After that, I changed the code that used the
previous functions and from this point I only used the shorthand that jQuery
offers, some examples may clarify this thought:

{% gist milmazz/ed5d4aab90ea70deda9c final.js %}

Another advantage of using the shorthand methods that jQuery provides is that
you can work with Deferreds, one last thing that we must take in consideration
is that `jqXHR.success()` and `jqXHR.error()` callback methods are deprecated
as of jQuery 1.8.

Anyway, I wanted to share my experience in this case. Also, remark that we need
to take care of some principles at the moment we develop software and avoid to
reinvent the wheel or do *overengineering*.

Last but not least, one way to read *offline* documentation that I tend to use is
[Dash][] or [Zeal][], I can reach a bunch of documentation without the need of an
Internet connection, give it a try!

[DRY]: http://en.wikipedia.org/wiki/DRY_principle
[jQuery]: http://jquery.com
[Dash]: http://kapeli.com/dash
[Zeal]: http://zealdocs.org
