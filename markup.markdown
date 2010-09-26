---
title:  "`static_gettext`: Marking up your documents for translation"
layout: default
---
Marking translatable strings
----------------------------

In order to localize a document, you'll need to tell `static_gettext`
which portions of the document ought be translated into target languages.

Let's take the following HTML document as an example:

    <!doctype html>
    <html lang="en">
      <head>
        <title>Hello, world!</title>
      </head>
      <body>
        <p>This is a document!</p>
      </body>
    </html>

Three strings would need translation if I wanted to render this page in German,
so let's demarcate them as translatable by wrapping them in `blocktrans` tags.

    <!doctype html>
    <html lang="{{ '{' }}% blocktrans %}en{{ '{' }}% endblocktrans %}">>
      <head>
        <title>{{ '{' }}% blocktrans %}Hello, world!{{ '{' }}% endblocktrans %}</title>
      </head>
      <body>
        <p>{{ '{' }}% blocktrans %}This is a document!{{ '{' }}% endblocktrans %}</p>
      </body>
    </html>

That's it.  There's not much more to it than that: `static_gettext` can now parse
the document, extract the marked strings, and [generate message files][Extraction].


I'd suggest sticking with the `{{ '{' }}% blocktrans %}TRANSLATABLE STRING{{ '{' }}% endblocktrans %}`
format, as it makes "upgrating" your static documents to a dynamic system
like Django trivial.  If, however, your needs dictate a different format,
the following are supported out of the box.  You can even mix and match
them, if you're willing to confuse yourself a bit:

*   `{{ '{' }}% blocktrans %}TRANSLATABLE STRING{{ '{' }}% endblocktrans %}`
*   `<blocktrans>TRANSLATABLE STRING</blocktrans>`
*   `<!-- blocktrans -->TRANSLATABLE STRING<!-- /blocktrans -->`
*   `/* blocktrans */TRANSLATABLE STRING/* /blocktrans */`

<ul class="usage" role="navigation">
  <li><a href="./install.html">Installation</a></li>
  <li><a href="./markup.html">Marking translatable strings</a></li>
  <li><a href="./extraction.html">Message file generation</a></li>
  <li><a href="./build.html">Generating target-language localizations</a></li>
</ul>

[Markup]:     markup.html
[Extraction]: extraction.html
[Build]:      build.html
[install]:  ./install.html
[example]:  http://github.com/mikewest/static_gettext/tree/master/example/
