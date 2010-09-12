Static `gettext`
===============

Django's internationalization framework is a relatively clean integration
of [`gettext`][gettext]'s approach to the tough problem of localizing
dynamic applications.  The process is straightforward:

1.  Text in an application's code and templates is marked for future
    translation, generally through `_('text')` function calls in code,
    and `{% trans ... %}` or `{% blocktrans %}...{% endblocktrans %}`
    tags in templates.

2.  Translatable strings are extracted from the application's source
    into _message files_ in a standard format.  These files contain
    each string (almost always in English) in the application , and
    allow translators to easily map them to their target-language
    counterparts.  One file is generated per language.

3.  Fully translated message files are then compiled into a binary
    format for quick lookup, and loaded into the application, where
    they can be used to dynamically generate responses in a language
    of the user's choice.

This is a good workflow, one which I'd like to replicate for static
documents and websites.  As you might have guessed, that's where this
repository comes in.  `static_gettext.py` wraps the `gettext` framework,
allowing static documents to be used as translation templates, generating
one copy of a set of documents for each language available.  From
`index.html`, you can produce `en_US/index.html`, `de_DE/index.html`, and
so on, enabling (among other things)  multi-language static websites.


[gettext]: http://www.gnu.org/software/gettext/

Usage
-----

The first step is to mark up the documents you'd like to translate.
`static_gettext.py` supports the following markup formats out of the
box:

*   `{% blocktrans %}TRANSLATABLE STRING{% endblocktrans %}`
*   `<blocktrans>TRANSLATABLE STRING</blocktrans>`
*   `<!-- blocktrans -->TRANSLATABLE STRING<!-- /blocktrans -->`
*   `/* blocktrans */TRANSLATABLE STRING/* /blocktrans */`

I prefer the former, as it makes "upgrading" to a real Django project
easy, but the others might be worthwhile, depending on your needs.

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
so let's demarcate them as translatable (see [`./example/src/index.html`][example_index]):

    <!doctype html>
    <html lang="{% blocktrans %}en{% endblocktrans %}">>
      <head>
        <title>{% blocktrans %}Hello, world!{% endblocktrans %}</title>
      </head>
      <body>
        <p>{% blocktrans %}This is a document!{% endblocktrans %}</p>
      </body>
    </html>

[example_index]: http://github.com/mikewest/static_gettext/blob/master/example/src/index.html

With those markers in place, I can generate message files:

    static_gettext.py --input ./example/src --locale ./example/locale --languages en_US,de_DE

This command generates [`./example/locale/en_US/LC_MESSAGES/messages.po`][example_po_en]
and [`./example/locale/de_DE/LC_MESSAGES/messages.po`][example_po_de], which both look
like:

    # SOME DESCRIPTIVE TITLE.
    # Copyright (C) YEAR THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # FIRST AUTHOR <EMAIL@ADDRESS>, YEAR.
    #
    #, fuzzy
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2010-09-12 16:34+0200\n"
    "PO-Revision-Date: YEAR-MO-DA HO:MI+ZONE\n"
    "Last-Translator: FULL NAME <EMAIL@ADDRESS>\n"
    "Language-Team: LANGUAGE <LL@li.org>\n"
    "Language: \n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"

    #: example/src/index.html:2
    msgid "en"
    msgstr ""

    #: example/src/index.html:4
    msgid "Hello, world!"
    msgstr ""

    #: example/src/index.html:7
    msgid "This is a document!"
    msgstr ""

[example_po_de]: http://github.com/mikewest/static_gettext/blob/master/example/locale/de_DE/LC_MESSAGES/messages.po
[example_po_en]: http://github.com/mikewest/static_gettext/blob/master/example/locale/en_US/LC_MESSAGES/messages.po

You should edit the header information of each `.po` file to reflect who's
actually responsible for each, then hand the file to that person for
translation.  That's as straightforward as you'd expect:

    ...
    
    #: example/src/index.html:2
    msgid "en"
    msgstr "de"

    #: example/src/index.html:4
    msgid "Hello, world!"
    msgstr "Hallo Welt!"

    #: example/src/index.html:7
    msgid "This is a document!"
    msgstr "Dies ist ein Dokument!"

    ...

`gettext` wants a compiled message file, so, run:

    static_gettext.py --compile --input ./example/src --locale ./example/locale --languages en_US,de_DE

That generates binary `.mo` files for both `en_US` and `de_DE`, which can be used
to generate translated versions of the file (see [`./example/build/en_US/index.html`][example_build_en] and [`./example/build/de_DE/index.html`][example_build_de]

    static_gettext.py --render --input ./example/src --locale ./example/locale --output ./example/build --languages en_US,de_DE

[example_build_de]: http://github.com/mikewest/static_gettext/blob/master/example/build/de_DE/index.html
[example_build_en]: http://github.com/mikewest/static_gettext/blob/master/example/build/en_US/index.html

In a nutshell:

    static_gettext.py --input ./path/to/input/root --locale ./path/to/locale/root --languages LANG1,LANG2,...
    # translate the message files in `./path/to/locale/root/[LANG]/LC_MESSAGES/`
    static_gettext.py --input ./path/to/input/root --locale ./path/to/locale/root --languages LANG1,LANG2,... --compile
    static_gettext.py --input ./path/to/input/root --locale ./path/to/locale/root --output ./path/to/build/root --languages LANG1,LANG2,... --render

The defaults are sensible, so you can leave off the `input`, `locale`, and
`output` arguments if you run the script from the root of a project 
laid out as follows:

    - project root
      - src
      - locale
      - build

The `example` directory in this project is, unsurprisingly, just such an example.

Notes
-----

*   By default, only files with `.html`, `.js`, `.css`, `.txt`, and
    `.markdown` extensions  are processed for translation.  Files with other
    extensions are simply copied over to the build directories.  This means
    you'll end up with multiple copies of images.  So far, I see this as an
    acceptable trade-off...

