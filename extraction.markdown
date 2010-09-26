---
title:  "`static_gettext`: Extract translatable strings into message files"
layout: default
---
Extract translatable strings into message files
-----------------------------------------------

After [marking up your document set][Markup], use `static_gettext` to 
extract the marked strings into message files for the project's target
languages:

    static_gettext.py --languages en_US,de_DE --make-messages

This command generates `locale/en_US/LC_MESSAGES/messages.po` and
`locale/de_DE/LC_MESSAGES/messages.po`, which, [in our example][example],
both look like:

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

Translation is a simple process of mapping `msgstr` values to `msgid` keys.
In the German localization, one such mapping would look like:

    #: example/src/index.html:4
    msgid "Hello, world!"
    msgstr "Hallo Welt!"

Longer strings can be written on one line, but are generally split across
multiple lines like this:

    #: example/src/index.html:4
    msgid "Hello, world!"
    msgstr ""
    "Ein Hallo-Welt-Programm ist ein kleines Computerprogramm und soll auf "
    "möglichst einfache Weise zeigen, welche Anweisungen oder Bestandteile "
    "für ein vollständiges Programm in einer Programmiersprache benötigt "
    "werden und somit einen ersten Einblick in die Syntax geben."

Special characters like linebreaks (or `"`) must be escaped in both the key 
and value:

    #: example/src/index.html:4
    msgid "Key"
    msgstr "Line 1\nLine 2\nQuote: \"\nLine 4"

Updating source documents
-------------------------

The framework is clever enough to do some fuzzy matching of previously
translated strings when you update source documents.  Simply run
`static_gettext.py --languages en_US,de_DE --make-messages` again after making changes,
and the message files will be regenerated: new strings will be added,
deleted strings will be commented out, unchanged strings will be retained
as-is, and lightly modified strings will reuse the previous translation,
but be marked `fuzzy`.  Searching for those strings after regenerating
message files is always a good idea, simply to ensure that the translation
still makes sense.

[Markup]:     markup.html
[Extraction]: extraction.html
[Build]:      build.html
[install]:  ./install.html
[example]:  http://github.com/mikewest/static_gettext/tree/master/example/
