---
redirect_from: "/archivos/2014/08/27/how-to-document-your-javascript-code/"
author: milmazz
comments: true
share: true
date: 2014-08-27T12:00:00-05:00
layout: post
slug: how-to-document-your-javascript-code
title: 'How to document your Javascript code'
description: Someone that knows something about Java probably knows about JavaDoc. If you know something about Python you probably document your code following the rules defined for Sphinx. Or in C, you follow the rules defined for Doxygen. But, what happens when we are coding in JavaScript? How can we document our source code?
tags:
- jsdoc
- javascript
- programming
---

Someone that knows something about Java probably knows about [JavaDoc][]. If you
know something about Python you probably document your code following the rules
defined for [Sphinx][] (Sphinx uses [reStructuredText][] as its markup
language). Or in C, you follow the rules defined for [Doxygen][] (Doxygen also
supports other programming languages such as Objective-C, Java, C#, PHP, etc.).
But, what happens when we are coding in JavaScript? How can we document our
source code?

As a developer that interacts with other members of a team, the need to document
all your intentions must become a habit. If you follow some basic rules and
stick to them you can gain benefits like the automatic generation of
documentation in formats like HTML, PDF, and so on.

I must confess that I'm relatively new to JavaScript, but one of the first
things that I implement is the source code documentation. I've been using
[JSDoc][] for documenting all my JavaScript code, it's easy, and you only need
to follow a short set of rules.

{% highlight javascript %}
/**
 * @file Working with Tags
 * @author Milton Mazzarri <me@milmazz.uno>
 * @version 0.1
 */

var Tag = $(function(){
  /**
   * The Tag definition.
   *
   * @param {String} id - The ID of the Tag.
   * @param {String} description - Concise description of the tag.
   * @param {Number} min - Minimum value accepted for trends.
   * @param {Number} max - Maximum value accepted for trends.
   * @param {Object} plc - The ID of the {@link PLC} object where this tag belongs.
   */
  var Tag = function(id, description, min, max, plc) {
    id = id;
    description = description;
    trend_min = min;
    trend_max = max;
    plc = plc;
  };

  return {
    /**
     * Get the current value of the tag.
     *
     * @see [Example]{@link http://example.com}
     * @returns {Number} The current value of the tag.
     */
    getValue: function() {
      return Math.random;
    }
  };
 }());
 {% endhighlight %}

In the previous example, I have documented the index of the file, showing the
author and version, you can also include other things such as a copyright and
license note. I have also documented the *class definition* including parameters
and methods specifying the name, and type with a concise description.

After you process your source code with [JSDoc][] the result looks like the
following:

![usejsdoc]({{ site.url }}/images/2014-08-27-how-to-document-your-javascript-code/jsdoc3.png)

In the previous image you see the documentation in HTML format, also you see a
table that displays the parameters with appropriate links to your source code,
and finally, JSDoc implements a very nice style to your document.

If you need further details I recommend you check out the [JSDoc documentation].

[JSDoc documentation]: http://usejsdoc.org/index.html
[JSDoc]: http://usejsdoc.org/
[JavaDoc]: http://en.wikipedia.org/wiki/Javadoc
[Doxygen]: http://www.stack.nl/~dimitri/doxygen/
[Sphinx]: http://sphinx-doc.org
[reStructuredText]: http://docutils.sf.net/rst.html
