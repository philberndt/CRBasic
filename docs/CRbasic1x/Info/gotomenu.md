# Goto Menu

Use **Goto** menu options to move the cursor to a different portion of the program. Note that many menu options also use the standard Windows shortcut keys. For a list of keyboard shortcuts, click the **Tools** menu and select **Show Keyboard Shortcuts**.

- **Navigation**: Programs have certain common instructions, such as the [BeginProg/EndProg](../Instructions/beginprogendprog.md) statements, the declaration of variables, data table definitions, subroutine and/or function declarations, and [Scan/NextScan](../Instructions/scannextscan.md). The Goto function is used to move the cursor to the next occurrence of a common instruction in the program.
  Select the **Navigation** option, and then select the common instruction from the list. Short cut keys (listed beside each instruction type) can be used instead of picking an instruction from the list, to quickly navigate to different sections of the program. Buttons for the navigation function are also available on the Toolbar.

- **Bookmarks**: Bookmarks are lines of code in the program that the user marks, which can be quickly navigated to using **Next Bookmark**, **Previous Bookmark**, and **Browse Bookmarks**. Clicking **Toggle Bookmark** adds a bookmark to a line. Clicking it a second time will remove the bookmark. When a line is bookmarked, the entire line is highlighted with a color (see [Syntax Highlighting](editorpreferences.md#Syntax) for instruction on customizing the appearance of bookmarks and other text). If you know the order in which you added bookmarks, you can also navigate between them using CTRL+# (for example, CTRL+5 will take you to bookmark 5, note that bookmark numbering starts at 0). Remove all bookmarks from a program by selecting **Clear Bookmarks**.

- **Go To Line**: Jump to a specific line number in a program. This is useful if the CRBasic compiler or the datalogger compiler returns a message such as "Error in Line Number 102."
