---
title:  "`static_gettext`: Localization for Static Documents"
layout: default
---
`static_gettext`<span class="offscreen">: </span>Localization for Static Documents
==================================================================================

<ul class="actions">
  <li><a href="http://github.com/mikewest/static_gettext/tarball/v0.9" class="cta">Download current version (v0.9)</a></li> 
  <li><a href="http://github.com/mikewest/static_gettext" class="cta">Project source on GitHub</a></li> 
</ul>

`static_gettext` provides an internationalization framework for static
documents and templates.  Implemented as a thin wrapper around the GNU
[gettext][] project, `static_gettext` produces standard "message files"
that any translator can easily work with.  After translation, these files
can be used to generate localizations for your world-wide audience.

[gettext]:  http://www.gnu.org/software/gettext/

Workflow
--------

1.  Text in an set of documents and templates is marked for future
    translation by wrapping translatable strings of text in 
    `{{ '{' }}%blocktrans %}...{{ '{' }}%endblocktrans %}` tags.

2.  These translatable strings are extracted from the source documents
    into _message files_ in a standard format.  These files contain
    each string to be translated, and allow translators to easily map
    them to their target-language counterparts. 

3.  For each localization, a message file is handed off to a translator
    who does the hard work of localizing the document.

4.  Fully translated message files are then compiled into a binary
    format for quick lookup, and used to generate localized versions
    of the document set.

Let's walk through each, step by step:

Markup
------

The first step is to mark up the documents you'd like to translate.
`static_gettext.py` supports the following markup formats out of the
box:

*   `{{ '{' }}%blocktrans %}TRANSLATABLE STRING{{ '{' }}%endblocktrans %}`
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
so let's demarcate them as translatable:

    <!doctype html>
    <html lang="{{ '{' }}%blocktrans %}en{{ '{' }}%endblocktrans %}">>
      <head>
        <title>{{ '{' }}%blocktrans %}Hello, world!{{ '{' }}%endblocktrans %}</title>
      </head>
      <body>
        <p>{{ '{' }}%blocktrans %}This is a document!{{ '{' }}%endblocktrans %}</p>
      </body>
    </html>

Easy.

Message File Generation
-----------------------

With those markers in place, I can generate message files:

    static_gettext.py --input ./example/src --locale ./example/locale --languages en_US,de_DE

This command generates `./example/locale/en_US/LC_MESSAGES/messages.po`
and `./example/locale/de_DE/LC_MESSAGES/messages.po`, which both look
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

And that's it for now.  Let's see how it looks.
