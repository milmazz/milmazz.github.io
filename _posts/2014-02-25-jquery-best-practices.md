---
title: jQuery best practices
author: milmazz
date: 2014-02-25 00:20:00
tags:
  - jquery
  - javascript
  - programming
slug: jquery-best-practices
redirect_from: /archivos/2014/02/25/jquery-best-practices/
---

After some time working with C programming language in a \*NIX like operating
system, recently I came back again to Web Programming, mostly working with HTML,
CSS and JavaScript in the client-side and other technologies in the backend
area.

The current project I've been working on, heavily relies on [jQuery][] and
[Highcharts/Highstock][HighCharts] libraries, I must confess that at the
beginning of the project my skills in the client-side were more than rusted, so,
I began reading a lot of articles about new techniques and good practices to
catch up very quickly, in the meantime, I start taking some notes about "best
practices"[^1] that promotes a better use of jQuery[^2]<!--more-->, I hope you
find this information useful, if you have more tips or any doubt please leave
your comments at the end of this article.

The first recommendation is to use the last version of jQuery, the reason is
that in new releases you will find new features, and also a good amount of bugs
and performance problems are fixed.

<!---
jquery 1.4.2 vs 1.6.2 vs 1.10.1
http://jsperf.com/dh-jquery-1-4-vs-1-6/50
-->

One of the benefits to work with jQuery is that it lets you find elements inside
of the DOM using a syntax similar to the CSS selectors[^3]. But, please, take
into account the following considerations.

Modern browser support `getElementsByClassName`, `querySelector` and
`querySelectorAll` directives. However, old browser versions only offer support
for `getElementById` and `getElementByTagName` directives. So, if you are only
targeting recent version of browsers, you might not need anything more than what
the browser offers.

One of the first tips that you need to know is this: Have preference over IDs
whenever it's possible, even in old browser versions the search based on the ID
of an element is fast because the jQuery selector internally is converted to the
native `getElementById` directive.

    {% highlight javascript %}
    $("#myelement"); // Good
    {% endhighlight %}

Avoid the tag prefix when you search for an `id`:

    {% highlight javascript %}
    $("div#myelement"); // Bad
    {% endhighlight %}

The example above creates an inefficient query because in the first place we
traverse all the `div` tags in the document, after that we look up the
`myelement` `id`.

In the same way, it's redundant to nest multiple IDs in a jQuery selector:

    {% highlight javascript %}
    $("#content #myelement"); // Bad
    {% endhighlight %}

In this case always use the most specific `id`.

    {% highlight javascript %}
    $("#myelement"); // Remember, keep it simple
    {% endhighlight %}

Also, if you want to select multiple elements, this implies that you need to
traverse all the DOM, which can generate a hit in your query performance, so,
you can avoid this negative impact descending from the most near ID element, for
example, imagine that you need to capture all the `input` elements inside of a
form which `id` is `#myform` (yes, I know, this ID name sucks, remember, it's
just for example purposes)

    {% highlight javascript %}
    $("#myform input") // Good
    $("#myform").find("input") // Better
    {% endhighlight %}

Avoid the selection of elements only by class
---------------------------------------------

Avoid to select an element only by its class, the major hit in performance
occurs when your browser doesn't support the native call
`getElementByClassName`, in old versions (like IE 6/7/8 and Mozilla Firefox 2)
jQuery needs to examine each element in the page to determine where is the class
that we are looking for.

    {% highlight javascript %}
    $(".myclass"); // Bad
    {% endhighlight %}

One workaround that you can use is to prefix the search with the tag element
associated with the class "myclass".

    {% highlight javascript %}
    $("div.myclass") // Good
    $("#container").find(".myclass") // Better
    {% endhighlight %}

But if you are targeting only modern browsers, you better try
`getElementsByClassName()`.

See: [jQuery class vs. tag qualified class selector](http://jsperf.com/jquery-class-vs-tag-qualfied-class-selector/25)

Cache
-----

Whenever you need to use multiple times the object returned by a jQuery
selector, please store that object in a variable, avoid the following practice:

    {% highlight javascript %}
    $("#intro").css("color", "blue");
    $("#intro").css("font-size", "1.2em");
    $("#intro").text("Hello World!");
    {% endhighlight %}

In this case you are making three queries to get the same object!, you can
improve the example above as follows:

    {% highlight javascript %}
    var $intro = $("#intro");
    $intro.css("color", "blue");
    $intro.css("font-size", "1.2em");
    $intro.text("Hello World!");
    {% endhighlight %}

In the example above we store the object (cache) returned by the jQuery
selector, after that we apply multiple methods over the same object.

Please note that the name of the local variable has the '$' prefix, that's not
really necessary, but some JavaScript developers have adopted this convention
that facilitates them to remember that local variable stores the results of a
jQuery selector. If you prefer you can avoid this "convention". The important
thing here is that if you need to apply more than one method over the same
object you must store this object in a local variable first.

Another advantage of storing objects in cache memory is that you can make sub-
queries, just look at the following example:

     {% highlight html %}
    <ul id="groceries">
        <li class="meat">Chicken</li>
        <li class="meat">Turkey breasts</li>
        <li class="oil">Olive oil</li>
        <li class="oil">Canola oil</li>
    </ul>
    {% endhighlight %}

And now we apply some queries over the unordered list:

    {% highlight javascript %}
    var $groceries = $("#groceries");
    {% endhighlight %}

After that we make some sub-queries:

    {% highlight javascript %}
    var $meat = $groceries.find("li.meat"),
        $oil = $groceries.find("li.oil");
    {% endhighlight %}

The principal benefit of store jQuery objects in memory is that the subsequent
queries don't need to traverse again the DOM.

See: [jQuery Cached Set](http://jsperf.com/ns-jq-cached)

Method chaining
---------------

Look at the following example:

    {% highlight javascript %}
    var $intro = $("#intro");
    $intro.css("color", "blue");
    $intro.css("font-size", "1.2em");
    $intro.text("Hello World!");
    {% endhighlight %}

Most of the jQuery methods returns an object, that *feature* facilitate the
method chaining over the same element:

    {% highlight javascript %}
    var $intro = $("#intro");
    $intro.css("color", "blue")
          .css("font-size", "1.2em")
          .text("Hello World!");
    {% endhighlight %}

See: [jQuery chaining](http://jsperf.com/jquery-chaining/32)

Group the set of parameters over the same method
------------------------------------------------

You can rearrange the previous example as this:

    {% highlight javascript %}
    $intro.css({"color": "blue", "font-size": "1.2em"})
          .text("Hello World!");
    {% endhighlight %}

This way you can reduce the number of times that you need to call the same
methods over the same objects. This technique create a unique literal object
that combines the set of parameters that we'll use to pass information to the
method.

Minimize the use of .append(), .insertBefore() and .insertAfter()
-----------------------------------------------------------------

If you want to use the `.append()` method, try to avoid its use inside a loop,
in the first place you can try to generate HTML strings in memory, after your
cycle ends you can append your new string to the content instead of apply the
`.append()` method inside your cycle.

Considering the following HTML structure:

    {% highlight html %}
    <ul id="demo">
        <!-- Content added via JS -->
    </ul>
    {% endhighlight %}

Please, avoid the following pattern:

    {% highlight javascript %}
    var $demo = $("#demo"),
      elements = ["one", "two", "three"];

    $.each(elements, function(i) {
      $("<li/>").text(elements[i])
                .appendTo($demo);
    });
    {% endhighlight %}

One approach can be this:

    {% highlight javascript %}
    var $demo = $("#demo"),
        elements = ["one", "two", "three"],
        string = "";

    for (var i=0, l=elements.length; i < l; i++) {
        string += "<li>" + elements[i] + "</li>";
    }

    $demo.append(string);
    {% endhighlight %}

Also, you can replace the `.append()` method with `.replaceWith()` in some
cases:

    {% highlight javascript %}
    var $demo = $("#demo"),
      elements = ["one", "two", "three"],
      string = "<ul id=\"demo\">";

    for (var i=0, l=elements.length; i < l; i++) {
      string += "<li>" + elements[i] + "</li>";
    }

    string += "</li>";

    $demo.replaceWith(string);
    {% endhighlight %}

Event delegation
----------------

Take this HTML structure as an example:

    {% highlight html %}
    <nav>
      <ul>
        <li id="home">Home</li>
        <li id="about">About</li>
        <li id="contact">Contact</li>
      </ul>
    </nav>
    {% endhighlight %}

One way to manage the navigation buttons are this:

    {% highlight javascript %}
    $("#home").click(function() {
      // do something
    });

    $("#about").click(function() {
        // do something
    });

    $("#contact").click(function() {
      // do something
    });
    {% endhighlight %}

This is really terrible because you bound an event to multiple DOM elements.

You can mitigate the example above as this:

    {% highlight javascript %}
    var $nav = $("nav");

    $nav.on("click", "#home", function() {
      // do something
    });

    $nav.on("click", "#about", function() {
      // do something
    });

    $nav.on("click", "#contact", function() {
      // do something
    });
    {% endhighlight %}

In the example above we use event delegation, this means that we need to bind to
a single element and intercept events as they bubble up the DOM tree.

If the previous methods do the same at the `click` event you can rearrange the
example above as this:

    {% highlight javascript %}
    var $nav = $("nav");

    $nav.on("click", "li", function() {
      // do something
    });
    {% endhighlight %}

Remember, the idea behind event delegation is to bind to a single root element
and then wait an intercept events as they *bubble up* the DOM tree.

Pseudo selectors
----------------

One the of the slowest jQuery selectors are *pseudo* and *attribute* selectors,
so, use this selectors with precaution.

    {% highlight javascript %}
    $(":visible, :hidden");
    $("[attribute=value]");
    {% endhighlight %}

The performance hit is greater in this cases because jQuery can't take advantage
of a native call. In modern browsers `querySelector()` and `querySelectorAll()`
can help us.

See: [id vs. class vs. tag vs. pseudo vs. attribute selectors](http://jsperf.com/id-vs-class-vs-tag-selectors/2)

$.each() or $fn.each() loops
----------------------------

Whenever you can, try to use a plain `for` (in some modern browsers `forEach` is
even faster than `for`) over the jQuery `$.each()` function. Also, you can gain
some benefits if you cache in memory the length of your container:

    {% highlight javascript %}
    // Good
    for (i = 0; i < values.length; i++) {
        // do something
    }
    // Better
    for (var i = 0, l = values.length; i < l; i++) {
        // do something
    }
    {% endhighlight %}

See: [Nesteds: JQuery each vs native JS for loop](http://jsperf.com/jquery-each-vs-for-loop/11)

References
-----------
 - [Efficient jQuery Selectors][] by Craig Buckler.
 - [jQuery Performance rules][] by ArtzStudio.
 - [jQuery best practices][] by Stephen A. Thomas.
 - [jQuery best practices](http://gregfranko.com/jquery-best-practices/) by Greg Franko.
 - [jQuery proven performance tips & tricks][] by Addy Osmani.
 - [DOM Core][]

[^1]: Please take this "best practices" with a grain of salt, do your research
and see if this recommendations applies in your web clients, in my case, I need
to provide support for some older browsers (IE 7/8/9 mostly).

[^2]: As a side note, besides jQuery nowadays are used by a large percentage of
sites, according to W3Techs, [jQuery now runs on every second website][] (2012).
However, currently exists a trend that states that you need to avoid jQuery when
it's possible, so, whenever it's possible try to use native JavaScript, one
example of this trend is [You Might Not Need jQuery][] website.

[^3]: You might find useful some other alternatives as [qwery][] and [Sizzle][].

[jQuery]: http://jquery.com/
[HighCharts]: http://www.highcharts.com/
[You Might Not Need jQuery]: http://youmightnotneedjquery.com/
[qwery]: https://github.com/ded/qwery
[Sizzle]: http://sizzlejs.com/
[Efficient jQuery Selectors]: http://www.sitepoint.com/efficient-jquery-selectors/
[jQuery Performance rules]: http://www.artzstudio.com/2009/04/jquery-performance-rules/
[jQuery best practices]: http://blog.sathomas.me/post/jquery-best-practices
[jQuery proven performance tips & tricks]: http://www.slideshare.net/AddyOsmani/jquery-proven-performance-tips-tricks
[DOM Core]: http://www.quirksmode.org/dom/w3c_core.html
[jQuery now runs on every second website]: http://w3techs.com/blog/entry/jquery_now_runs_on_every_second_website
