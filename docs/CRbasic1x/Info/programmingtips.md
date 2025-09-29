# Programming Tips

The CRBasic Editor has been designed with several features to assist in making program creation and editing easier.

## Inserting Instructions

An instruction can be easily inserted into the program by highlighting it in the Instruction Panel list [(seeCRBasic Editor)](crbasiceditor.md) and clicking **Insert**. If an instruction has one or more parameters, a Parameter dialog box is displayed. Complete the information in the parameter fields and click ** Insert**to insert the instruction into the program.

When the Parameter window is opened, you can move to the next instruction in the program by clicking ** Next**at the bottom of the window (or click ** Prev**to move back to the previous instruction). If a change has been made in the current instruction, you are asked whether to save or discard the change before moving to the next instruction.

## Right-Click Functionality

The result of a right-click action varies, depending upon your cursor location.

- Right-click on an instruction to display the Parameter dialog box and edit instruction parameters.

- Right-click on a parameter that uses a Variable as an input type to display a list of variables that have been defined in the program. Variable list:

  The variable list is sorted by variable type and then alphabetically by name. In the previous list, the first green A denotes that the variable`AirTemp3ft`is set up as an [Alias](../Instructions/alias.md).[Constants](../Instructions/const1.md) are listed with a blue C,[Dimensioned](../Instructions/dim.md) variables are listed with a red D, and [Public](../Instructions/public.md) variables are listed with a black P. Variables that are Dimensioned within a subroutine or function, and thus local variables, are listed with a purple L.

  At any time, you can press F10 to display the list of variables, regardless of your cursor location.

- Right-click on a parameter that has a finite number of valid entries to display a list of available options.

- Right-click on a parameter that does not fall within the previous two categories to display help for that parameter.

- Right-click on a block of text that is highlighted to display the following options:
  - **Comment **/** Uncomment Block **: Only one of these options is available, depending upon the status of the highlighted text. If the text has been marked as a Comment, you can choose to uncomment it. If the text is not commented, you can choose to make it into a comment. Keyboard shortcuts for** Comment **/** Uncomment **are** Ctrl **+**'** and **Shift**+** Ctrl **+**'**, respectively. Commented text has a single quote ( ' ) at the beginning of the line. Comments are ignored by the datalogger's compiler.
  - ** Decrease **/** Increase Indent **: You can increase or decrease the indentation of the selected text using one of these menu options. The spacing is increased or decreased by either a tab or one or more spaces, depending upon the settings set in the** View **menu,** Editor Preferences **,** Editor **tab,** Auto Indenting **section. Keyboard shortcuts for** Decrease **/** Increase Indent **are** Ctrl+[**and ** Ctrl+]**, respectively.
  - ** Cut **/** Copy **/** Paste **/** Delete **: Standard editing functions.
  - ** Save as .CRB File **- Saves the highlighted text to a new file with a CRB extension. This is referred to as a library file. The file can then be inserted into a program using the** Insert File **option (right-click on a blank line and select** Insert File **from the floating menu, or click the** Edit **menu and select** Insert File **).
  - Note that .CRB is also the [Program File Extension](Program%20File%20Extension.md) used for Granite Dataloggers.

## Line Continuation

Line continuation allows an instruction or logical line to span one or more physical lines. This allows you to break up long lines of code into more readable chunks . Line continuation is indicated by one white space character that immediately precedes a single underscore character as the last character of a line of text. Following is an example of line continuation:

Public Temp, RH, WindSp, WindDir, \_

BatteryV, IntRH, IntTemp, RainTot, \_

RainInt, Solar

A single line of code is limited to 512 characters (the buffer for a line of program code is 512 bytes). A "line of code" includes code that is placed on multiple lines using line continuation syntax.

## Syntax Highlighting

The CRBasic Editor, by default, highlights different types of elements in the program using different font styles and colors. Comments, Instruction Names, and other text elements each have a different appearance. You can customize or completely disable syntax highlighting by changing your Editor Preferences (click the** View **menu, select** Editor Preferences **, set options on the** Syntax Highlighting **tab).

## Keyboard Shortcuts

Many of the items available from the menu can be accessed using Function Keys or keyboard shortcuts(such as** Ctrl+S **for** Save **,** F10 **for a Variable list,** F11 **for a user-defined function list, etc.). Click the** Tools **menu and select** Show Keyboard Shortcuts**for a full list of keystrokes that can be used in the program.

## Help System

The CRBasic Editor is provided with an extensive help system. For more information on using the help system refer to [Accessing the Help System](accessingthehelpsystem.md).
