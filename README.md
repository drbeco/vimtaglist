# vimtaglist

## Plugin

This plugin repository is based on the Yegappan plugin.

For now, it is just the exact original with no modifications, uploaded in `git` history one by one, so we can use a plugin manager to install it.

The overview of the file is in the source itself, replicated here (bellow) for easier reading.

Happy hacking.

Att.

Dr. Bèco.

## Source Code Introduction

```
" File: taglist.vim
" Author: Yegappan Lakshmanan (yegappan AT yahoo DOT com)
" Version: 2.5
" Last Modified: April 26, 2003
"
" Overview
" --------
" The "Tag List" plugin provides an overview of the structure of source code
" files and allows you to efficiently browse through source code files in
" different programming languages. The "Tag List" plugin provides the
" following features:
"
" 1. Opens a vertically/horizontally split Vim window with a list of tags
"    (functions, classes, structures, variables, etc) defined in the current
"    file.
" 2. Groups the tags by their type and displays them in a foldable tree.
" 3. Automatically updates the taglist window as you switch between
"    files/buffers.
" 4. When a tag name is selected from the taglist window, positions the cursor
"    at the definition of the tag in the source file
" 5. Automatically highlights the current tag name.
" 6. Can display the prototype of a tag from the taglist window.
" 7. Displays the scope of a tag.
" 8. Can optionally use the tag prototype instead of the tag name.
" 9. The tag list can be sorted either by name or by line number.
" 10. Supports the following language files: Assembly, ASP, Awk, Beta, C, C++,
"     C#, Cobol, Eiffel, Erlang, Fortran, HTML, Java, Javascript, Lisp, Lua,
"     Make, Pascal, Perl, PHP, Python, Rexx, Ruby, Scheme, Shell, Slang, SML,
"     Sql, TCL, Verilog, Vim and Yacc.
" 11. Runs in all the platforms where the exuberant ctags utility and Vim are
"     supported (this includes MS-Windows and Unix based systems).
" 12. Runs in both console/terminal and GUI versions of Vim.
" 13. The ctags output for a file is cached to speed up displaying the taglist
"     window.
" 14. Works with the winmanager plugin. Using the winmanager plugin, you can
"     use Vim plugins like the file explorer, buffer explorer and the taglist
"     plugin at the same time like an IDE.
" 15. Can be easily extended to support new languages. Support for existing
"     languages can be modified easily.
"
" To see the screenshots of the taglist plugin in different environments,
" visit the following page:
"
"       http://www.geocities.com/yegappan/taglist/screenshots.html
"
" This plugin relies on the exuberant ctags utility to dynamically generate
" the tag listing. You can download the exuberant ctags utility from
"
"               http://ctags.sourceforge.net
"
" The exuberant ctags utility must be installed in your system to use this
" plugin. You should use exuberant ctags version 5.0 and above.  This plugin
" doesn't use or create a tags file and there is no need to create a tags file
" to use this plugin.
"
" This plugin relies on the Vim "filetype" detection mechanism to determine
" the type of the current file. You have to turn on the Vim filetype detection
" by adding the following line to your .vimrc file:
"
"               filetype on
"
" This plugin will not work in 'compatible' mode.  Make sure the 'compatible'
" option is not set. This plugin will not work if you run Vim in the
" restricted mode (using the -Z command-line argument). This plugin also
" assumes that the system() Vim function is supported.
"
" Installation
" ------------
" 1. Copy the taglist.vim plugin to the $HOME/.vim/plugin directory. Refer to
"    ':help add-plugin', ':help add-global-plugin' and ':help runtimepath' for
"    more details about Vim plugins.
" 2. Set the Tlist_Ctags_Cmd variable to point to the location of the
"    exuberant ctags utility (not to the directory).
" 3. If you are running a terminal/console version of Vim and the terminal
"    doesn't support changing the window width then set the Tlist_Inc_Winwidth
"    variable to 0.
" 4. Restart Vim.
" 5. You can use the ":Tlist" command to open/close the taglist window. 
"
" Usage
" -----
" You can open the taglist window by using the ":Tlist" command. Invoking this
" command will toggle (open or close) the taglist window. You can map a key to
" invoke this command. For example, the following command creates a normal
" mode mapping for the <F8> key to open or close the taglist window.
"
"               nnoremap <silent> <F8> :Tlist<CR>
"
" Add the above mapping to your ~/.vimrc file.  You can also open the taglist
" window on startup using the following command line:
"
"               $ vim +Tlist
"
" You can close the taglist window from the taglist window by pressing 'q' or
" using the Vim ":q" command. You can also use any of the Vim window commands
" to close the taglist window. Invoking the ":Tlist" command when the taglist
" window is opened, will close the taglist window.
"
" As you switch between source files, the taglist window will be automatically
" updated with the tag listing for the current source file.  The tag names
" will grouped by their type (variable, function, class, etc). For tags with
" scope information (like class members, structures inside structures, etc),
" the scope information will be displayed in square brackets "[]" after the
" tagname.
" 
" The tag names will be  displayed as a foldable tree using the Vim folding
" support. You can collapse the tree using the '-' key or using the Vim zc
" fold command. You can open the tree using the '+' key or using the Vim zo
" fold command. You can open all the fold using the '*' key or using the Vim
" zR fold command You can also use the mouse to open/close the folds.
"
" You can select a tag either by pressing the <Enter> key or by double
" clicking the tag name using the mouse. You can configure the taglist plugin
" by setting the 'Tlist_Use_SingleClick' variable to jump to a tag on a single
" mouse click.
"
" This plugin will automatically highlight the name of the current tag.  The
" tag name will be highlighted after 'updatetime' milliseconds. The default
" value for this Vim option is 4 seconds.  You can also use the ":TlistSync"
" command to force the highlighting of the current tag. You can map a key to
" invoke this command. For example, the following command creates a normal
" mapping for the <F9> key to highlight the current tag name.
"
"               nnoremap <silent> <F9> :TlistSync<CR>
"
" Add the above mapping to your ~/.vimrc file.
"
" If you place the cursor on a tag name in the "Tag List" window, then the tag
" prototype will be displayed at the Vim status line after 'updatetime'
" milliseconds. The default value for the 'updatetime' Vim option is 4
" seconds. You can also press the space bar to display the prototype of the
" tag under the cursor.
"
" By default, the tag list will be sorted by the order in which the tags
" appear in the file. You can sort the tags either by name or by order by
" pressing the "s" key in the taglist window.
"
" You can press the 'x' key in the taglist window to maximize the taglist
" window width/height. The window will be maximized to the maximum possible
" width/height without closing the other existing windows. You can again press
" 'x' to restore the taglist window to the default width/height.
"
" You can press the '?' key to display help information about using the
" taglist window. If you again press the '?' key, the help information will be
" removed.
"
" The following table lists the description of the keys that you can use
" in the taglist window.
"
"       Key           Description
"
"       <CR>          Jump to the location where the tag under cursor is
"                     defined.
"       o             Jump to the location where the tag under cursor is
"                     defined in a new window.
"       <Space>       Display the prototype of the tag under the cursor.
"       u             Update the tags listed in the taglist window
"       s             Change the sort order of the tags (by name or by order)
"       x             Zoom-in or Zoom-out the taglist window
"       +             Open a fold
"       -             Close a fold
"       *             Open all folds
"       q             Close the taglist window
"       ?             Display help
"
"
" You can use the ":TlistShowPrototype" command to display the prototype of
" a function in the specified line number. For example,
"
"               :TlistShowPrototype 50
"
" If the line number is not supplied, this command will display the prototype
" of the current function.
"
" You can also use the taglist plugin with the winmanager plugin. This will
" allow you to use the file explorer, buffer explorer and the taglist plugin
" at the same time in different windows. To use the taglist plugin with the
" winmanager plugin, set 'TagList' in the 'winManagerWindowLayout' variable.
" For example, to use the file explorer plugin and the taglist plugin at the
" same time, use the following setting:
"
"               let winManagerWindowLayout = 'FileExplorer|TagList'
"
" Configuration
" -------------
" By changing the following variables you can configure the behavior of this
" plugin. Set the following variables in your .vimrc file using the 'let'
" command.
"
" This plugin uses the Tlist_Ctags_Cmd variable to locate the ctags utility.
" By default, this is set to ctags. Set this variable to point to the location
" of the ctags utility in your system. Note that this variable should point to
" the fully qualified exuberant ctags location and NOT to the directory in
" which exuberant ctags is installed.
"
"               let Tlist_Ctags_Cmd = 'd:\tools\ctags.exe'
"               let Tlist_Ctags_Cmd = '/usr/local/bin/ctags'
"
" By default, the tag names will be listed in the order in which they are
" defined in the file. You can alphabetically sort the tag names by pressing
" the "s" key in the taglist window. You can also change the default order by
" setting the variable Tlist_Sort_Type to "name" or "order":
"
"               let Tlist_Sort_Type = "name"
"
" Be default, the tag names will be listed in a vertically split window.  If
" you prefer a horizontally split window, then set the
" 'Tlist_Use_Horiz_Window' variable to 1. If you are running MS-Windows
" version of Vim in a MS-DOS command window, then you should use a
" horizontally split window instead of a vertically split window.  Also, if
" you are using an older version of xterm in a Unix system that doesn't
" support changing the xterm window width, you should use a horizontally split
" window.
"
"               let Tlist_Use_Horiz_Window = 1
"
" By default, the vertically split taglist window will appear on the left hand
" side. If you prefer to open the window on the right hand side, you can set
" the Tlist_Use_Right_Window variable to one:
"
"               let Tlist_Use_Right_Window = 1
"
" To automatically open the taglist window, when you start Vim, you can set
" the Tlist_Auto_Open variable to 1. By default, this variable is set to 0 and
" the taglist window will not be opened automatically on Vim startup.
"
"               let Tlist_Auto_Open = 1
"
" By default, only the tag name will be displayed in the taglist window. If
" you like to see tag prototypes instead of names, set the
" Tlist_Display_Prototype variable to 1. By default, this variable is set to 0
" and only tag names will be displayed.
"
"               let Tlist_Display_Prototype = 1
"
" The default width of the vertically split taglist window will be 30.  This
" can be changed by modifying the Tlist_WinWidth variable:
"
"               let Tlist_WinWidth = 20
"
" Note that the value of the 'winwidth' option setting determines the minimum
" width of the current window. If you set the 'Tlist_WinWidth' variable to a
" value less than that of the 'winwidth' option setting, then Vim will use the
" value of the 'winwidth' option.
"
" By default, when the width of the window is less than 100 and a new taglist
" window is opened vertically, then the window width will be increased by the
" value set in the Tlist_WinWidth variable to accommodate the new window.  The
" value of this variable is used only if you are using a vertically split
" taglist window.  If your terminal doesn't support changing the window width
" from Vim (older version of xterm running in a Unix system) or if you see any
" weird problems in the screen due to the change in the window width or if you
" prefer not to adjust the window width then set the 'Tlist_Inc_Winwidth'
" variable to 0.  CAUTION: If you are using the MS-Windows version of Vim in a
" MS-DOS command window then you must set this variable to 0, otherwise the
" system may hang due to a Vim limitation (explained in :help win32-problems)
"
"               let Tlist_Inc_Winwidth = 0
"
" By default, when you double click on the tag name using the left mouse 
" button, the cursor will be positioned at the definition of the tag. You 
" can set the Tlist_Use_SingleClick variable to one to jump to a tag when
" you single click on the tag name using the mouse. By default this variable
" is set to zero.
"
"               let Tlist_Use_SingleClick = 1
"
" Due to a bug in Vim, if you set Tlist_Use_SingleClick to one and try to
" resize the taglist window using the mouse, then Vim will crash. The fix for
" this bug will be available in the next version of Vim. In the meantime,
" instead of resizing the taglist window using the mouse, you can use normal
" Vim window resizing commands to resize the taglist window.
"
" By default, the taglist window will contain text that display the name of
" the file, sort order information and the key to press to get help. Also,
" empty lines will be used to separate different groups of tags. If you
" don't need these information, you can set the Tlist_Compact_Format variable
" to one to get a compact display.
"
"               let Tlist_Compact_Format = 1
"
" Extending
" ---------
" You can extend exuberant ctags to add support for new languages. For more
" information, visit the following page
"
"               http://ctags.sourceforge.net/EXTENDING.html
"
" You can extend the taglist plugin to add support for new languages or modify
" the support for an already supported language by setting the following
" variables in the .vimrc file.
"
" To modify the support for an already supported language, you have to set the
" tlist_xxx_settings variable. Replace xxx with the Vim filetype name.  To
" determine the filetype name used by Vim for a file, use the command
"
"               :set filetype
"
" The format of the value set in the tlist_xxx_settings variable is
"
"          <language_name>;flag1:name1;flag2:name2;flag3:name3
"
" The different fields are separated by the ';' character.  The first field
" 'language_name' is the name used by exuberant ctags. This name can be
" different from the file type name used by Vim. For example, for C++, the
" language name used by ctags is 'c++' but the filetype name used by Vim is
" 'cpp'. The remaining fields follow the format "flag:name". The sub-field
" 'flag' is the language specific flag used by exuberant ctags to generate the
" corresponding tags.  For example, for the C language, to list only the
" functions, the 'f' flag should be used. For more information about the flags
" supported by exuberant ctags for a particular language, read the help text
" from the 'ctags --help' comand. The sub-field 'name' specifies the title
" text to use for displaying the tags of a particular type. For example,
" 'name' can be set to 'functions'.
"
" For example, to list only the classes and functions defined in a C++
" language file, add the following lines to your .vimrc file
"
"       let tlist_cpp_settings = 'c++;c:class;f:function'
"
" In the above setting, 'cpp' is the Vim filetype name and 'c++' is the name
" used by the exuberant ctags tool. 'c' and 'f' are the flags passed to
" exuberant ctags to list classes and functions.
"
" For example, to display only functions defined in a C file and to use "My
" Functions" as the title for the function group, use 
"
"       let tlist_c_settings = 'c;f:My Functions'
"
" To add support for a new language, set the tlist_xxx_settings variable
" appropriately as described above.
"
```

* Original versions can be found at https://vim.sourceforge.io/scripts/script.php?script_id=273

## Installation

Use `vim-plug` manager and add:

```
Plug 'drbeco/vimtaglist'
```

## Maintainer 

* Ruben Carlo Benante, aka Dr. Bèco
* Email: rcb at beco dot cc
* License MIT


