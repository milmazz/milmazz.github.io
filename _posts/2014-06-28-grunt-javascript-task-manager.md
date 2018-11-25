---
redirect_from: "/archivos/2014/06/28/grunt-javascript-task-manager/"
author: milmazz
comments: true
share: true
date: 2014-06-28T16:00:00-05:00
layout: post
slug: grunt-javascript-task-manager
title: 'Grunt: The Javascript Task Manager'
description: When you play the Web Developer role, sometimes you may have to endure some repetitive tasks like minification, unit testing, compilation, linting, beautify or unpack JavaScript code and so on. To solve this problems, and in the meantime, try to keep your mental health in a good shape, you desperately need to find a way to automate this tasks. [Grunt][] offers you an easy way to accomplish this kind of automation.
excerpt_separator: <!--more-->
tags:
- grunt
- javascript
- programming
---

When you play the Web Developer role, sometimes you may have to endure some
repetitive tasks like minification, unit testing, compilation, linting,
beautify or unpack Javascript code and so on. To solve this problems, and in
the meantime, try to keep your mental health in a good shape, you desperately
need to find a way to automate this tasks. [Grunt][] offers you an easy way to
accomplish this kind of automation.

<!--more-->

In this article I'll try to explain how to automate some tasks with Grunt, but
I recommend that you should take some time to read Grunt's documentation and
enjoy the experience by yourself.

So, in the following sections I'll try to show you how to accomplish this
tasks:

 - Concatenate and create a minified version of your CSS, JavaScript and HTML
files.
 - Automatic generation of the documentation for JavaScript with JSDoc.
 - Linting your JavaScript code.
 - Reformat and reindent (prettify) your JavaScript code.

You can install [Grunt][] via [npm][] (Node Package Manager), so, to
install Grunt you need to install [Node.js][Node] first.

Now that you have [Node.js][Node] and `npm` installed is a good time to install
*globally* the Grunt CLI (Command Line Interface) package.

{% highlight sh %}
$ sudo npm install -g grunt-cli
{% endhighlight %}

Once `grunt-cli` is installed you need to go to the root directory of your
project and create a `package.json` file, to accomplish this you can do the
following:

{% highlight sh %}
$ cd example_project
$ npm init
{% endhighlight %}

The previous command will ask you a series of questions in order to create the
`package.json` file, `package.json` basically store metadata for projects
published as `npm` modules. It's important to remember to add this file to your
source code versioning tool to ease the installation process of the development
dependencies among your partners via `npm install` command.

At this point we can install Grunt and their respective plugins in the
existing `package.json` with:

{% highlight sh %}
$ npm install grunt --save-dev
{% endhighlight %}

And the plugins that you need can be installed as follows:

{% highlight sh %}
$ npm install <grunt-plugin-name> --save-dev
{% endhighlight %}

Please note that the `--save-dev` parameter will change your `devDependencies`
section in your `package.json`. So, be sure to commit the updated `package.json`
file with your project whenever you consider appropriate.

Code documentation
------------------

If you document your code following the syntax rules defined on [JSDoc][] 3,
e.g.:

{% highlight javascript %}
 /**
  * A callback function returning array defining where the ticks
  * are laid out on the axis.
  *
  * @function tickPositioner
  * @see {@link http://api.highcharts.com/highstock#yAxis.tickPositioner}
  * @param {String} xOrY - Is it X or Y Axis?
  * @param {Number} min - Minimum value
  * @param {Number} max - Maximum value
  * @returns {Array} - Where the ticks are laid out on the axis.
  */
 function tickPositioner(xOrY, min, max) {

    // do something

    return tickPositions;
 }
{% endhighlight %}

If you need more information about [JSDoc][], read their documentation, it's
easy to catch up.

The next step to automate the generation of the code documentation is to install
first the [grunt-jsdoc][grunt-jsdoc] plugin as follows:

{% highlight sh %}
$ npm install grunt-jsdoc --save-dev
{% endhighlight %}

Once `grunt-jsdoc` is installed you must create your `Gruntfile.js` in the
root directory of your project and then add the `jsdoc` entry to the options
of the `initConfig` method.

{% highlight javascript %}
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
      pkg: grunt.file.readJSON('package.json'),
      jsdoc : {
          dist : {
              src: ['src/*.js', 'test/*.js'],
              dest: 'doc'
          }
      }
  });

};
{% endhighlight %}

Then, you need to load the plugin after the `initConfig` method in the
`Gruntfile.js`:

{% highlight javascript %}
// Load the plugin that provides the 'jsdoc' task.
grunt.loadNpmtasks('grunt-jsdoc');
{% endhighlight %}

The resulting `Gruntfile.js` until now is:

{% highlight javascript %}
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
      pkg: grunt.file.readJSON('package.json'),
      jsdoc : {
          dist : {
              src: ['src/*.js', 'test/*.js'],
              dest: 'doc'
          }
      }
  });

  // Load the plugin that provides the 'jsdoc' task.
  grunt.loadNpmtasks('grunt-jsdoc');

};
{% endhighlight %}

To generate the documentation, you need to call the `jsdoc` task as follows:

{% highlight sh %}
$ grunt jsdoc
{% endhighlight %}

Immediately you can see, inside the `doc` directory, the available documentation
in HTML format with some beautiful styles by default.

![JSDoc]({{ site.url }}/images/grunt/jsdoc.png)

Linting your JavaScript code
----------------------------

In order to find suspicious, non-portable or potential problems in JavaScript
code or simply to enforce your team's coding convention, whatever may be the
reason, I recommend that you should include a static code analysis tool in
your toolset.

The first step is to define your set of rules. I prefer to specify the set of
rules in an independent file called `.jshintrc` located at the root directory of
the project, let's see an example:

{% highlight javascript %}
// file: .jshintrc
{
    "globals": {
        "Highcharts": true, // This is only necessary if you use Highcharts
        "module": true // Gruntfile.js
    },
    "bitwise": true,
    "browser": true,
    "camelcase": true,
    "curly": true,
    "eqeqeq": true,
    "forin": true,
    "freeze": true,
    "immed": true,
    "indent": 4,
    "jquery": true,
    "latedef": true,
    "newcap": true,
    "noempty": true,
    "nonew": true,
    "quotmark": true,
    "trailing": true,
    "undef": true,
    "unused": true
}
{% endhighlight %}

If you need more details about the checks that offer every rule mentioned above,
please, read the page that contains a list of
[all options supported by JSHint][JSHint]

To install the [grunt-contrib-jshint][] plugin, , please do as follows:

{% highlight sh %}
$ npm install grunt-contrib-jshint --save-dev
{% endhighlight %}

Next, proceed to add the `jshint` entry to the options of the `initConfig`
method in the `Gruntfile.js` as follows:

{% highlight javascript %}
jshint: {
 options: {
  jshintrc: '.jshintrc'
 },
 all: ['Gruntfile.js', 'src/*.js', 'test/*.js']
}
{% endhighlight %}

Then, as it's done in the previous section, you need to load the plugin after
the `initConfig` method in the `Grunfile.js`

{% highlight javascript %}
// Load the plugin that provides the 'jshint' task.
grunt.loadNpmTasks('grunt-contrib-jshint');
{% endhighlight %}

To validate your JavaScript code against the previous set of rules you need to
call the `jshint` task:

{% highlight sh %}
$ grunt jshint
{% endhighlight %}

Note that if you need more information or explanations about the errors that you
may receive after the previous command I suggest that you should visit
[JSLint Error Explanations][jslinterrors] site.

Code Style
----------

As I mentioned in the previous section, I suggest that you should maintain an
independent file where you define the set of rules about your coding styles:

{% highlight javascript %}
// file: .jsbeautifyrc
{
  "indent_size": 4,
  "indent_char": " ",
  "indent_level": 0,
  "indent_with_tabs": false,
  "preserve_newlines": true,
  "max_preserve_newlines": 2,
  "jslint_happy": true,
  "brace_style": "collapse",
  "keep_array_indentation": false,
  "keep_function_indentation": false,
  "space_before_conditional": true,
  "break_chained_methods": false,
  "eval_code": false,
  "unescape_strings": false,
  "wrap_line_length": 0
}
{% endhighlight %}

Next, proceed to add the `jsbeautifier` entry to the options of the `initConfig`
method in the `Gruntfile.js` as follows:

{% highlight javascript %}
jsbeautifier: {
  modify: {
      src: 'index.js',
      options: {
          config: '.jsbeautifyrc'
   }
  },
  verify: {
   src: ['index.js'],
   options: {
   mode: 'VERIFY_ONLY',
   config: '.jsbeautifyrc'
  }
 }
}
{% endhighlight %}

The next step it to load the plugin after the `initConfig` method:

{% highlight javascript %}
// Load the plugin that provides the 'jsbeautifier' task.
grunt.loadNpmTasks('grunt-jsbeautifier');
{% endhighlight %}

To adjust your JS files according to the previous set of rules you need to call
the `jsbeautifier` task:

{% highlight sh %}
$ grunt jsbeautifier:modify
{% endhighlight %}

Concat and minified CSS, HTML, JS
---------------------------------

To reduce the size of your CSS, HTML and JS files do as follows:

{% highlight sh %}
$ npm install grunt-contrib-uglify --save-dev
$ npm install grunt-contrib-htmlmin --save-dev
$ npm install grunt-contrib-cssmin --save-dev
{% endhighlight %}

Next, add the `htmlmin`, `cssmin` and `uglify` entries to the options
of the `initConfig` method in the `Gruntfile.js` as follows:

{% highlight javascript %}
htmlmin: {
  dist: {
    options: {
      removeComments: true,
      collapseWhitespace: true
    },
    files: {
      'dist/index.html': 'src/index.html',     // 'destination': 'source'
      'dist/contact.html': 'src/contact.html'
    }
  }
},
cssmin: {
  add_banner: {
    options: {
      banner: '/* My minified css file */'
    },
    files: {
      'path/to/output.css': ['path/to/**/*.css']
    }
  }
},
uglify: {
 options: {
   banner: '/* <%= grunt.template.today("yyyy-mm-dd") %> */\n',
   separator: ',',
   compress: true,
 },
 chart: {
   src: ['src/js/*.js'],
   dest: 'dist/js/example.min.js'
 }
}
{% endhighlight %}

The next step it to load the plugins after the `initConfig` method:

{% highlight javascript %}
// Load the plugin that provides the 'uglify' task.
grunt.loadNpmTasks('grunt-contrib-uglify');
// Load the plugin that provides the 'htmlmin' task.
grunt.loadNpmTasks('grunt-contrib-htmlmin');
// Load the plugin that provides the 'cssmin' task.
grunt.loadNpmTasks('grunt-contrib-cssmin');
{% endhighlight %}

To adjust your files according to the previous set of rules you need to call
the `htmlmin`, `cssmin` or `uglify` tasks:

{% highlight sh %}
$ grunt htmlmin
$ grunt cssmin
$ grunt uglify
{% endhighlight %}

After loading all of your Grunt tasks you can create some aliases.

If you want to create an alias that runs the three previous tasks in one step do
as follows:

{% highlight javascript %}
// Wrapper around htmlmin, cssmin and uglify tasks.
grunt.registerTask('minified', [
        'htmlmin',
        'cssmin',
        'uglify'
    ]);
{% endhighlight %}

So, to execute the three tasks in one step you can do the following:

{% highlight sh %}
$ grunt minified
{% endhighlight %}

The previous command is a wrapper around the `htmlmin`, `cssmin` and `uglify`
tasks defined before.

Wrapping all together we get the following [Grunfile.js][]

Other plugins
-------------

Grunt offers you a very large amount of [plugins][], they have a dedicated
section to list them!

I recommend that you should take some time to check out the following
*plugins*:

- [grunt-newer][] lets you configure Grunt tasks to run with newer files
only.
- [grunt-contrib-watch][] lets you run predefined tasks whenever file patterns
are added, changed or deleted.
- [grunt-contrib-imagemin][] let you minify images.

Conclusion
----------

Certainly, Grunt do a great job when we talk about task automation, also,
automating this repetitive tasks is fun and reduce the errors associated with
tasks that in the past we used to run manually over and over again. That been
said, I definitely recommend that you should try Grunt, you will get in exchange
an improvement in your development workflow, also with it you can forget to
endure repetitive tasks that we hate to do, as a result you will get more
free time to focus on getting things done.

Last but not least, another option for Grunt is [Gulp][], I recently read about
it, if you look at their documentation you notice that their syntax is more
clean than Grunt, also is very concise and allows chaining calls, following the
concept of streams that pipes offer in *nix like systems, so, I'll give it a try
in my next project.

[Grunt]: http://gruntjs.com
[Node]: http://nodejs.org
[JSDoc]: http://usejsdoc.org
[npm]: https://www.npmjs.org
[grunt-jsdoc]: https://github.com/krampstudio/grunt-jsdoc
[jslinterrors]: http://jslinterrors.com
[JSHint]: http://www.jshint.com/docs/options/
[grunt-contrib-jshint]: https://github.com/gruntjs/grunt-contrib-jshint
[Gulp]: http://gulpjs.com
[plugins]: http://gruntjs.com/plugins
[Gruntfile.js]: https://gist.github.com/milmazz/46a14b4694ff0aaf7250
[grunt-newer]: https://www.npmjs.org/package/grunt-newer
[grunt-contrib-watch]: https://www.npmjs.org/package/grunt-contrib-watch
[grunt-contrib-imagemin]: https://www.npmjs.org/package/grunt-contrib-imagemin
