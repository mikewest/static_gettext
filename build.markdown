---
title:  "`static_gettext`: Generate localizations after translation"
layout: default
---
Generate localizations after translation
----------------------------------------

After [generating and translating message files][Extraction], use
`static_gettext` to generate localizations by running the following
command from the project root:

    static_gettext --languages LANG_1,LANG_2,... --build

That generates binary `*.mo` versions of the `*.po` message files,
and then uses those compiled files to quickly run through all the 
files in your `src` directory to generate translations in the target
languages, which will be placed in the `build/[TARGET]/` directory.

Two things are important to note:

1.  By default, only files with `.html`, `.js`, `.css`, `.txt`, `.xml` and
    `.markdown` extensions  are processed for translation.  If your
    source documents include translatable plaintext files with other
    extensions, you can specify those via a `--extensions` flag.

2.  Files that don't match the list of valid plaintext extensions are
    copied to the build directory.  This means that you'll end up with
    multiple copies of images, binaries, etc.  So far, I see this as an 
    acceptable trade-off, but I'm considering symlinks for the future.    


[Markup]:     markup.html
[Extraction]: extraction.html
[Build]:      build.html
[install]:  ./install.html
[example]:  http://github.com/mikewest/static_gettext/tree/master/example/

