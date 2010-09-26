---
title:      "`static_gettext`: Localization for Static Documents"
layout:     default
bodyclass:  homepage
---

<ul class="actions">
  <li><a href="http://github.com/mikewest/static_gettext/tarball/v0.9" class="cta">Download current version (v0.9)</a></li> 
  <li><a href="http://github.com/mikewest/static_gettext" class="cta">Source code on GitHub</a></li> 
</ul>

<img src="workflow.png" width="565" height="380" alt="static_gettext in a nutshell">

`static_gettext` is an internationalization framework for static, plaintext
documents and templates.  It's geared towards straightforward translation
of static websites, but can be easily used for any set of files you'd like
to translate as a group.

Implemented as a thin wrapper around the GNU [gettext][] project,
`static_gettext` produces standard _message files_ that any translator can
easily work with.  After translation, these files can be used to generate
localizations for your world-wide audience.

[gettext]:  http://www.gnu.org/software/gettext/

Quickstart
----------

After [installing the script][install], getting up and running is trivial.
Let's take a quick look at the [example project][example] to get our bearings:

*   **[Markup][]**: Documents to be localized are placed into the project's `src`
    directory, and marked up for future translation by wrapping strings of
    translatable text in `{{ '{' }}% blocktrans %}...{{ '{' }}% endblocktrans %}`
    tags.

*   **[Extraction][]**: These translatable strings are extracted from the source
    documents into _message files_ in standard `gettext` format.  A single
    message file containing all translation strings is generated for each
    target language as `locale/[TARGET]/LC_MESSAGES/messages.po` by running
    the following command from the project root:

        static_gettext.py --languages LANG_1,LANG_2,... --make-messages

*   **[Build][]**: After translation, the `*.po` files are updated with the
    localized strings and compiled into a `*.mo` binary format for quick
    lookup.  These binary files are then used to generate localized versions
    of the project as `build/[TARGET]/*` by running the following from the
    project root:

        static_gettext.py --languages LANG_1,LANG_2,... --build

Detailed Usage
--------------

[Markup]:     markup.html
[Extraction]: extraction.html
[Build]:      build.html
[install]:  ./install.html
[example]:  http://github.com/mikewest/static_gettext/tree/master/example/
