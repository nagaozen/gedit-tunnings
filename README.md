gEdit Tunnings
==============

gEdit tunnings holds my personal setup to make the editor work my way. Anyone 
interested in using gEdit for VBScript, Javascript, Python, Ruby, MSSQL, C# and 
others may check this repository for small tips and tricks.

Enable Menu Shortcut keys
-------------------------

This is a great tip! GNOME let you to change the menu shortcuts really easily! 
Using Ubuntu, you should:

    Click System > Preferences > Appearance
    Click Interface tab
    Check "Enable menu shortcut keys"

After this, you can set any shortcut to any menu just focusing the menu option
and typing the keys you want to use as shortcut. To rollback, just use `backspace`.

External Tools
--------------

### Hyphenate

Description: **Hyphenates a sentence**  
Shortcut Key: `<Control><Alt>h`  
Commands:  
    #!/usr/bin/env python

    import sys

    print sys.stdin.read().replace(" ", "-").lower()
Input: **Current Selection**  
Output: **Replace the current selection**  
Applicability: **All documents**
