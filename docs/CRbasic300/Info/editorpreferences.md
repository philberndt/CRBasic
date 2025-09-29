# Setting Editor Preferences

In the CRBasic Editor, click the **View** menu and select **Editor Preferences** to view and personalize CRBasic Editor settings, and to set spacing and syntax highlighting options

## Editor Tab

Click the **View** menu, select **Editor Preferences**, and click the ** Editor**tab to enable or disable CRBasic Editor features.

### Parameter Hint Box

Parameter hints provide information about a parameter when you hover over a parameter for a period of time. Clear the ** Show Parameter Hints**check box to turn the hints off. Click the increase/decrease buttons next to the ** Hint Delay**box to set the amount of time to hover over a parameter before the hint is displayed (the default is 1000ms). Click ** Select Color**to change the background color used for the hint.

### Auto Indenting

By default, the CRBasic Editor automatically indents instructions after`Scan`or`SubScan`instructions, and after conditional statements. This helps to identify the flow of the program. You can choose to indent using a defined number of **Spaces** or using **Tabs** by selecting the desired option button. If the **Spaces** option is chosen, indicate the number of spaces to use for the indention in the **Spaces** field. If the **Tabs** option is chosen, the width of the tab is determined by **Tab Width** field under the **Tab Key Mode** settings.

### Tab Key Mode

These options determine what characters are used when the tab key is pressed. Select **Use Tab** to insert a tab character into the code when the **Tab** key is pressed. Select **Use Spaces** to insert a fixed number of spaces, rather than a tab, when the **Tab** key is pressed. Type a number in the **Tab Width** box to indicate the number of spaces to use for the indention.

### Edit Features

- **Enable Drag-n-Drop **: By default, text can be repositioned in the program by highlighting it and "dragging and dropping" it to a new location. Disable the drag and drop feature by clearing the** Enable Drag-n-Drop **check box.

- ** Enable Duplicate Name Check **: The CRBasic Editor checks for duplicate variables as you edit the program. If a duplicate name is found, an error is displayed in the Error Message section of the window. Disable duplicate variable checking by clearing the** Enable Duplicate Name Check **check box.

- ** Enable Keyword Capitalization **: Program keywords are capitalized automatically, based on the capitalization used in the Instruction Panel list. Disable keyword capitalization by clearing the** Enable Keyword Capitalization **check box.

- ** Enable Variable Name Match **: Variable names are capitalized based on how they are declared in the program, regardless of how the user may have typed them (an auto-correct feature for variable name capitalization). Disable variable name capitalization by clearing the** Enable Variable Name Match **check box.

- ** Create TDF File at Compile**:When a program is compiled for aCR300datalogger a`program_name.TDF`file is created along with the original program file. (TDF stands for Table Definitions File) This file contains the table definitions (table size, variable names, data types, etc.) for that program. In software that supports this functionality, the user can associate aTDF

  **Note:** Table Definitions File

  file with a datalogger. This can be useful if communication is taking place over a slow or unreliable communications link where the attempt to receive table definitions back from the datalogger fails. Disable creation of the TDF file by clearing the Create .TDF File at Compilecheck box.

- **Clear Undo/RedoList on File Save **: Changes are tracked in the program (used with Undo/Redo) when the file is saved. To the clear the change tracking information when file is closed, clear the** Clear Undo/RedoList on File Save **check box.

- ** Enable Display of Right Margin and Pagination Lines **: Enables the display of margin and pagination lines in the Editor. If you find these lines interfere with reading text on the screen, disable them by clearing the** Enable Display of Right Margin and Pagination Lines **check box.

- ** Check for Unused Variables/Subroutines at Compile **: A warning message is displayed at compile time if any variables or subroutines are declared in the program but are not used. To disable the display of the warning message, clear the** Check for Unused Variables/Subroutines at Compile **check box.

- ** Enable Line Numbering **: Line numbers in the Editor are displayed by default. To disable the display of line numbers, clear the** Enable Line Numbering **check box. Click the browse buttons next to** Font Color **and** Background Color **to select a font and background color for the line numbers.

## Vertical Spacing Tab

At any time during editing a program, you can choose** Edit **and click** Rebuild Indentation **(short cut is CTRL + i) to have the CRBasic Editor automatically correct spacing and indenting throughout the program. The Vertical Spacing tab is used to determine which instructions will have spacing before them or after them, and how the CRBasic Editor should process blank lines.

## Syntax Highlighting

You can customize the appearance of the text elements in the Editor from the Syntax Highlighting tab. A different font style and color can be defined for Normal text, keywords, comments, operators, numbers, parentheses, strings, and bookmarks, making the program easier to read and edit. Click the** View **menu, select** Editor Preferences **, and click the** Syntax Highlighting **tab to customize the appearance of the text elements in the Editor. Define font style and color for numerous program text types to make the program easier to read and edit. To disable syntax highlighting, clear the** Enable Syntax Highlighting**check box.
