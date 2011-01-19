gEdit Tunnings
==============

gEdit tunnings holds some personal setup to make the editor work my way. Anyone 
interested in using gEdit for VBScript, Javascript, Python, Ruby, MSSQL, C# etc
may check this repository for small tips and tricks.

Keyboard Shortcuts
------------------

The official list of keyboard shortcuts is maintained at [live.gnome.org/gedit/keyboardshortcuts](http://live.gnome.org/Gedit/KeyboardShortcuts "gedit keyboard shortcuts")

Enhanced Editing features
-------------------------

> Note  i: More information about gtkrc-2.0 signals available at [GTKTextView reference](http://library.gnome.org/devel/gtk/unstable/GtkTextView.html "GTKTextView reference")  
> Note ii: More information about gtkrc-2.0 binding available at [GTK+ Bindings](http://library.gnome.org/devel/gtk/2.21/gtk-Bindings.html "GTK+ Bindings")

### Duplicate line

Bind `<Control><Shift>D` to duplicate the current line.

Add the following lines to your **.gtkrc-2.0** file (`$ gedit ~/.gtkrc-2.0`)

    # Duplicate line
    binding "control-shift-d" {
        bind "<Control><Shift>d" {
            "move-cursor" (paragraph-ends, -1, 0)
            "move-cursor" (paragraph-ends, 1, 1)
            "copy-clipboard" ()
            "move-cursor" (paragraph-ends, 1, 0)
            "insert-at-cursor" ("\n")
            "paste-clipboard" ()
        }
    }
    class "GeditView" binding :highest "control-shift-d"

### Raise line

Bind `<Control>Up` to raise the current line.

Add the following lines to your **.gtkrc-2.0** file (`$ gedit ~/.gtkrc-2.0`)

    binding "control-up" {
        bind "<Control>Up" {
            "move-cursor" (paragraph-ends, -1, 0)
            "move-cursor" (display-lines, 1, 1)
            "cut-clipboard" ()
            "move-cursor" (display-lines, -1, 0)
            "paste-clipboard" ()
            "move-cursor" (display-lines, -1, 0)
        }
    }
    class "GeditView" binding :highest "control-up"

### Lower line

Bind `<Control>Down` to lower the current line.

Add the following lines to your **.gtkrc-2.0** file (`$ gedit ~/.gtkrc-2.0`)

    # Lower line
    binding "control-down" {
        bind "<Control>Down" {
            "move-cursor" (paragraph-ends, -1, 0)
            "move-cursor" (display-lines, 1, 1)
            "cut-clipboard" ()
            "move-cursor" (display-lines, 1, 0)
            "paste-clipboard" ()
            "move-cursor" (display-lines, -1, 0)
        }
    }
    class "GeditView" binding :highest "control-down"

### Some tips for people trying to make their own bindings

    # Tips:
    # "move-cursor" (visual-positions, -1, 0) previous char
    # "move-cursor" (visual-positions, 1, 0) next char
    # "move-cursor" (words, -1, 0) moves to the start of the word or previous word
    # "move-cursor" (words, 1, 0) moves to the end of the word or next word
    # "move-cursor" (display-lines, -1, 0) moves to the previous line (trying to keep the cursor in the same position)
    # "move-cursor" (display-lines, 1, 0) moves to the next line (trying to keep the cursor in the same position)
    # "move-cursor" (display-line-ends, -1, 0) moves to the line start (absolute|non-indented)
    # "move-cursor" (display-line-ends, 1, 0) moves to the line end (absolute|with-spaces)
    # "move-cursor" (paragraphs, -1, 0) moves to the previous line start
    # "move-cursor" (paragraphs, 1, 0) moves to the next line (with content) end
    # "move-cursor" (paragraph-ends, -1, 0) moves to the line start ABSOLUTE
    # "move-cursor" (paragraph-ends, 1, 0) moves to the line end ABSOLUTE

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

There is a lot of color schemes (themes) available in this [github repository](http://github.com/mig/gedit-themes/)  
and a lot more at [gmate styles folder](https://github.com/gmate/gmate/tree/master/styles).

Plugins
-------

### Official

    sudo apt-get install gedit-plugins

### Third party

* [AutoComplete](http://github.com/nagaozen/gedit-plugin-autocomplete/)
* [Class Browser](http://github.com/nagaozen/gedit-plugin-classbrowser/)
* [Quick Highlight Mode](http://github.com/nagaozen/gedit-plugin-quickhighlightmode/)
* TODO List -- see [gmate](https://github.com/gmate/gmate).  
> Note: gMate has a lot of things, but **use it carefully - a lot of plugins in this package should be deprecated in favor of official ones and others
> are simply out-of-date**. I strongly recommend you to keep track of individual plugins for the last and update version.
* [Zen Coding](https://github.com/mikecrittenden/zen-coding-gedit)

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

    cmd = "js beautify-cl.js -i 4 %s"%(tmpfile)
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
