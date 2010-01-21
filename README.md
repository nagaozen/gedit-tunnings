gEdit Tunnings
==============

gEdit tunnings holds some personal setup to make the editor work my way. Anyone 
interested in using gEdit for VBScript, Javascript, Python, Ruby, MSSQL, C# etc
may check this repository for small tips and tricks.

Enable Menu Shortcut keys
-------------------------

This is a great tip! GNOME let you to change the menu shortcuts really easily! 
Using Ubuntu, you should:

    Click System > Preferences > Appearance
    Click Interface tab
    Check "Enable menu shortcut keys"

After this, you can set any shortcut to any menu just focusing the menu option
and typing the keys you want to use as shortcut. To rollback, just use `<backspace>`.

Envy Code R
-----------

For a programmer, a good monospaced font is vital just as the editor. I'm currently using the
[Envy Code R by damieng](http://damieng.com/blog/2008/05/26/envy-code-r-preview-7-coding-font-released), 
a well thought TTF created by a VS programmer. It's very easy to install:

    Download the font package (view the author page)
    Extract the package to ~/.fonts/
    $ fc-cache
    
    Click System > Preferences > Appearance
    Click Fonts tab
    Set "Fixed width font" to "Envy Code R"
    
    Open gedit
    Click Edit > Preferences
    Click Font & Colors tab
    Check "Use the system fixed width font"

Color Schemes
-------------

There are a lot of color schemes (themes) available in this [github repository](http://github.com/mig/gedit-themes/).

Plugins
-------

### Official

    sudo apt-get install gedit-plugins gedit-latex-plugin

### Third party

* [AutoComplete](http://github.com/nagaozen/gedit-plugin-autocomplete/)
* [Class Browser](http://github.com/nagaozen/gedit-plugin-classbrowser/)
* [Multi-edit](http://svn.jon-walsh.com/multi-edit/)
* [Pastie](http://github.com/ivyl/gedit-pastie)
* [Quick Highlight Mode](http://github.com/nagaozen/gedit-plugin-quickhighlightmode/)
* TODO List -- see [gMate](http://github.com/lexrupy/gmate/).  
> Note: gMate has a lot of things, but **use it carefully - a lot of plugins
> in this package should be deprecated in favor of official ones and others
> are simply out-of-date**. I strongly recommend you to keep track of individual
> plugins for the last and update version.

Configure the External Tools plugin
-----------------------------------

> Note: If you fork and add a new external tool to this collection, please keep the sorting by the `H3` name.

### Compute selection length

#### Entry:

Description: **Computes selection length**  
Shortcut Key:  <Control>l
Commands:  
    #!/usr/bin/env python

    import sys

    print "Current Selection has %s characters"%(len(sys.stdin.read()))
Input: **Current selection**  
Output: **Display in bottom pane**  
Applicability: **All documents**

--------------------------------------------------------------------------------

### Diff

#### Requirements:

    $ sudo apt-get install zenity meld

#### Entry:

Description: **Diff the current document against another one**  
Shortcut Key: `<Control><Alt>d`  
Commands:  
    #!/bin/sh

    meld $GEDIT_CURRENT_DOCUMENT_DIR/$GEDIT_CURRENT_DOCUMENT_NAME `zenity --file-selection --title="External Tools - Diff against ..."` &
Input: **Nothing**  
Output: **Display in the bottom pane**  
Applicability: **Local files only**

--------------------------------------------------------------------------------

### Format as HTML 4.01

#### Requirements:

    $ sudo apt-get install tidy

#### Entry:

Description: **Tidy to HTML 4.01**  
Shortcut Key:  
Commands:  
    #!/bin/sh

    exec tidy -utf8 -ashtml -wrap 0 -indent -upper -quiet "$GEDIT_CURRENT_DOCUMENT_NAME"
Input: **Nothing**  
Output: **Create new document**  
Applicability: **Local files only**

--------------------------------------------------------------------------------

### Format as XHTML 1.0

#### Requirements:

    $ sudo apt-get install tidy

#### Entry:

Description: **Tidy to XHTML 1.0**  
Shortcut Key:  
Commands:  
    #!/bin/sh

    exec tidy -utf8 -asxhtml -wrap 0 -indent -quiet "$GEDIT_CURRENT_DOCUMENT_NAME"
Input: **Nothing**  
Output: **Create new document**  
Applicability: **Local files only**

--------------------------------------------------------------------------------

### Format Javascript

#### Requirements:

    $ sudo apt-get install rhino

This tool also requires [einars jsbeautifier](http://github.com/einars/js-beautify "jsbeautifier").
Just set `jsbeautify_path` in the `Commands` to the right place.

#### Entry:

Description: **Beautify Javascript using einars jsbeautify**  
Shortcut Key:  
Commands:  
    #!/usr/bin/env python

    import os
    import sys
    import tempfile

    jsbeautify_path = "/home/nagaozen/Development/js-beautify/"

    content = sys.stdin.read()
    h, tmpfile = tempfile.mkstemp()
    os.close(h)

    f = open(tmpfile, "w")
    f.write(content)
    f.close()

    cmd = "java org.mozilla.javascript.tools.shell.Main beautify-cl.js -i 4 %s"%(tmpfile)
    os.chdir(jsbeautify_path)
    content = os.system(cmd)
    os.remove(tmpfile)

    print content
Input: **Current Selection**  
Output: **Replace the current selection**  
Applicability: **All documents**

--------------------------------------------------------------------------------

### Insert Lipsum paragraph

#### Requirements:

Download and Install (lorem-ipsum-generator](http://code.google.com/p/lorem-ipsum-generator/).

#### Entry:

Description: **Inserts a Lipsum paragraph at cursor**  
Shortcut Key: `<Control><Alt>i`  
Commands:  
    #!/bin/sh

    lorem-ipsum-generator -p 1
Input: **Nothing**  
Output: **Insert at cursor position**  
Applicability: **All documents**

--------------------------------------------------------------------------------

### Hyphenate

#### Entry:

Description: **Hyphenates a sentence**  
Shortcut Key: `<Control><Alt>h`  
Commands:  
    #!/usr/bin/env python

    import sys
    import unicodedata

    def not_combining(char):
        return unicodedata.category(char) != "Mn"

    def strip_accents(text, encoding):
        unicode_text = unicodedata.normalize('NFD', text.decode(encoding))
        return filter(not_combining, unicode_text).encode(encoding)

    print strip_accents(sys.stdin.read(), "UTF-8").lower().replace(' ', '-')
Input: **Current Selection**  
Output: **Replace the current selection**  
Applicability: **All documents**

--------------------------------------------------------------------------------

### Parse a LESS document

#### Requirements:

    $ sudo apt-get install ruby rubygems
    sudo gem install less

#### Entry:

Description: **LESSC the current document**  
Shortcut Key:  
Commands:  
    #!/usr/bin/env ruby

    require 'rubygems'
    require 'less'

    puts Less.parse($stdin.read)
Input: **Current Document**  
Output: **Create new document**  
Applicability: **All documents**

Configure the Snippets plugin
-----------------------------

    $ sudo mkdir /usr/share/gedit-2/plugins/snippets/builtin-snippets
    $ sudo mv /usr/share/gedit-2/plugins/snippets/*.xml /usr/share/gedit-2/plugins/snippets/builtin-snippets
    $ cp -rf <this snippets folder> ~/.gnome2/gedit
