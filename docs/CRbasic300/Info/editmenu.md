# Edit Menu

Use **Edit** menu options to make programming more efficient. Note that most of these options also use the standard Windows shortcut keys. For a list of keyboard shortcuts, click the **Tools** menu and select **Show Keyboard Shortcuts**.

- ** Undo **: Cancels the last keystroke or action.

- ** Redo **: Reverses the last "Undo" operation.

- ** Undo Multiple **: Displays a list of all keystrokes and actions that can be "undone" until the list is cleared. Changes are listed in the reverse order that they were made, with the most recent change as the first entry in the list. A single change or a range of changes can be selected to "undo". See [Working with Instruction Categories](editinstructioncategories.md) to set when the change tracking list is cleared.

- ** Redo Multiple **: Reverses the last "Undo Multiple" operation.

- ** Cut **: Removes the selected text and places it on the Windows clipboard. This option is unavailable if no text is selected( shortcut Ctrl+X).

- ** Copy **: Places a copy of the selected text on the Windows clipboard. This option is unavailable if no text is selected (shortcut Ctrl+C).

- ** Paste **: Pastes a copy of the Windows clipboard contents at the cursor location (shortcut Ctrl+V).

- ** Delete **: Deletes the selected text (shortcut Ctrl+DEL).

- ** Select All **: Selects all text in the CRBasic file (shortcut Ctrl+A).

- ** Create Compressed File**: Creates a new file with an`_str`extension. All user comments and line spacing in the program are removed from the file. Removing comments and spaces can signficantly reduce the file size in larger programs.[SeeCreating a Compressed Filefor more information.](sendingaprogram.md#Compress)

- **Rebuild Indentation **: Reworks code indentation and removes blank lines based on the Vertical Spacing rules [(seeVertical Spacing Tabfor more information)](editorpreferences.md#Vertical).

- ** Save As CRB**: Saves selected text to a file with a`*.CRB`extension. This file is referred to as a "library file". The file can then be reused by inserting it into another CRBasic program using the **Edit** menu,**Insert File ** option.

- **Insert File**: Inserts a library file into the current program at the location of the cursor.
