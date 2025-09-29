# Compile Menu

When a program is compiled, the CRBasic Editor checks the program for syntax errors and other inconsistencies. The results of the check are displayed in a message window at the bottom of the main window. If an error can be traced to a specific line in the program, the line number is listed before the error. You can double-click an error preceded by a line number and that line is highlighted in the program editing window. To move the highlight to the next error in the program, click the **Next Error** buttonor click the **Compile** menu and select **Next Error**. To move the highlight to the previous error in the program, click the ** Previous Error**buttonor choose ** Previous Error**from the ** Compile**menu.

The error window can be closed by clicking the ** View**menu and selecting ** Close Message Window**or by clickingin the upper right corner of the compiler messages area.

** Compile **: Compiles the program. The file is not saved prior to compiling.

** Save and Compile **: Save the program file and run it through the compiler to check for errors.

** Compile, Save and Send **: Compile the program, save it to disk, and send it to a selected datalogger. Note, the Connect screen may display a message that communication has been disabled. This is because compiling the new program causes the datalogger to reset. Thus, the state of the open connection is lost and the connection is closed. If an automatic clock check goes out when this occurs, the Connect screen detects the problem and disables communication. To reduce the occurrence of this error message and disconnect, pause the automatic clock check in the Connect window.[See alsoSending a Program to the Datalogger.](sendingaprogram.md)

** Conditional Compile and Save**: Generate a new CRBasic program from code that uses conditional compile syntax (`#If/#Else/#ElseIf`statements) or [Constant Customization](constantcustomization.md). For detailed information, see [Conditional Compilation](conditionalcompilation.md).

| When a program is compiled that uses conditional syntax, any conditional compilation statements that do not evaluate as true are removed from the program and the program is compiled. When a program is compiled that uses constant customization, the constant values selected in the **Tools | Customize Constants** menu item are used when compiling the new program. In either instance, you are prompted to save the file under a user-specified name or the file is saved under the name of the original program with \_CC# appended. The # is a number that increments to create a unique filename. For instance, if the program name is`myprogram.CRB`, the first time it is compiled the default name is`myprogram_CC1.CRB`. If`myprogram_CC1.CRB`exists, the program is named`myprogram_CC2.CRB`. |

The`#If_no_remove`declaration can be used in place of`#If`to specify conditional code that should not be removed from the resulting program when a Conditional Compile and Save is performed.

**Conditional Compile, Include Files and Save ** Generate a new CRBasic program with Include files expanded. The code from the Include file is inserted in the program wherever the Include statement resides. If the Include file is encrypted or if the file is not found in the same directory in which the program is being precompiled in CRBasic, the Include file will not be expanded. All other rules for conditional compile syntax, as stated in the previous**Conditional Compile and Save** section, are followed; i.e._,_ any conditional compilation statements that do not evaluate as true are removed from the program before the program is compiled. You are prompted to save the file under a user-specified name or the file is saved under the name of the original program with \_CC# appended. The # is a number that increments to create a unique filename. For instance, if the program name is`myprogram.cr1`, the first time it is compiled the default name is`myprogram_CC1.CRB`. If`myprogram_CC1.CRB`exists, the program is named`myprogram_CC2.CRB`.

**Pick CR200 Compiler**- When this menu option is selected, the CRBasic Editor searches for and displays a list of all CR200 compilers installed on the computer in a specific directory. From this list you can choose the compiler to use when the program file is compiled and the binary (\*.bin) file is created. It is important that the compiler being used match the operating system version in the datalogger. If a different compiler is used, the program send will fail.

**NOTE:** When a CR200 program is compiled, the binary program file is created that is loaded into to the datalogger. For all other dataloggers, the compile serves as a syntax checking tool only.

## TDF Files

TDF stands for Table Definitions File. When a program is compiled for a GRANITE-series, CR6, CR3000, CR1000X, CR800-series, CR300-series, or CR1000 datalogger, a`program_name.TDF`file is created along with the original program file. This file contains the table definitions (table size, variable names, data types, etc.) for that program. In software that supports this functionality, the user can associate aTDF

**Note:** Table Definitions File

file with a datalogger. This can be useful if communication is taking place over a slow or unreliable communications link where the attempt to receive table definitions back from the datalogger fails.

This function can be turned off by clicking the **View** menu, selecting **Editor Preferences**, and clearing the ** Create TDF File at Compile**check box [(seeSetting Editor Preferencesfor more information)](editorpreferences.md).
