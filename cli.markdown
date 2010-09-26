---
title:  "`static_gettext`: Command line usage"
layout: default
---
Command Line Usage
------------------

<pre><code>Usage: static_gettext.py [options]

Options:

  -h, --help            show this help message and exit

  -d DOMAIN, --domain=DOMAIN
                        The localization's 'domain' (used to create the
                        message file's name)

  -m, --make-messages   Generate message files in target languages

  -b, --build           Build localizations in target languages

  -l DIR, --locale=DIR  The directory where the .po/.mo files ought be located

  -o DIR, --output=DIR  The directory into which translated files ought be
                        rendered.
                      
  -i DIR, --input=DIR   The directory from which to read the translation
                        templates.
                        
  -e .EXT1,.EXT2,.EXT3,..., --extensions=.EXT1,.EXT2,.EXT3,...
                        File extensions which ought to be parsed for
                        translatable strings (Defaults to
                        `.html,.js,.css,.txt,.xml,.markdown`)
                        
  --languages=LANG1,LANG2,LANG3,...
                        A comma seperated list of the languages in which
                        translations should be made. (Defaults to
                        `en_US,de_DE`)</code></pre>
