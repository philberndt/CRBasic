# CRBasic Program Structure

CRBasic programs are similar in form to structured BASIC programs. Following is the typical layout of a program. Note that each instruction in this help file has example code to demonstrate the use of the instruction in a program. Look for the Example link at the top of each instruction.

## Program Declarations [(seeDeclarationsfor more information)](../Instructions/declarations.md)

**Declare Constants **: Assign variable names to fixed constants.[For more information, seeConst (Assign a Name to a Value).](../Instructions/const1.md)

** Declare Variables **: Variables declared using the [Public](../Instructions/public.md) instruction are available for viewing on the datalogger's display and within software. Variables declared using the [Dim](../Instructions/dim.md) instruction are not available for viewing. "Scratch" variables and variables that are not important for the user to monitor are often defined with the Dim instruction.

** Define Aliases **: Assign second names to previously defined variables.[For more information, seeAlias.](../Instructions/alias.md)

### ** Variable Notes and Rules** Variables for the CR200(X) can be up to 16 characters in length. However, if the variable is processed in the output table by an output type other than Sample, the name is truncated in the datalogger to 12 characters, plus an underscore and a 3 digit suffix indicating the output type (for example, \_avg, \_max). This could result in a "duplicate field name" error when the program is compiled, if two or more variables are output with the same first 12 characters.

Variables for the GRANITE-series, CR6, CR3000, CR1000X, CR800-series, CR300-series, CR1000, CR5000\*, and CR9000(X)\* can be up to 39 characters in length. With these dataloggers, the suffix containing the output type (for example, \_avg) is appended to the end of the variable name. Therefore, to stay within the 39 character limit, most variables should be no more than 35 characters (which allows for the 4 additional characters that may be needed for output processing identifiers). For a list of the suffixes used for each output type in the dataloggers, see [Output Instruction Suffixes](outputinstructionsuffixes1.md).

\* For CR5000 and CR9000(X), requires OS with 2011 release date. Otherwise, limit is 16 characters.

The CR200 has a limit of 48 variables that can be defined. The CR200(X) has a limit of 128 variables.

Valid characters for use in variable names include the letters A through Z (upper and lower case), the underscore character ( \_ ), the dollar sign ($), and numbers 0 through 9. Variable names must start with a letter, an underscore, or a dollar sign. Variable names are not case sensitive.

Variables declared by Dim within a subroutine or function are local to that subroutine or function. The same variable name can be used within other subroutines or functions or as a global variable without conflict. Variables used as parameters to a subroutine or function are also local. The array length of a local variable in a subroutine will take on the array length of a global variable passed into the local variable. All variables declared by Public, even within a subroutine or function, are global.

CRBasic dataloggers do not have predefined user flags like mixed array dataloggers. In CRBasic, a flag is simply a declared variable. In datalogger support software (for example, LoggerNet, PC400), if a Public variable is dimensioned with the name of Flag(), the number of flags are displayed in the Ports/Flags window of the software.

## Data Tables

**Define Process/Store Trigger **: Set when the data should be stored:[conditional](../Instructions/dataevent.md) or [fixed interval](../Instructions/datainterval.md)

** NOTE:**CR200 series dataloggers store data only on a fixed interval.

** Set Table Size **: Set how many records are stored in a table before new data overwrites old.

** Define Other Online Storage Devices **: Should data be sent to an external storage device?

** Process Data **: Define output processing: sample, average, maximum, minimum, etc.

[SeeAbout Data Tablesfor more information.](datatables.md)

** NOTE:**Computer operating system reserved words such as NUL, CON, AUX, COM1, PRN, LPT1, etc. should be avoided in table names.

CR200 dataloggers can have only 4 data tables defined. CR200X dataloggers can have up to 8 data tables defined.

## Subroutines

Define any process or series of calculations that might be repeated several times in the program.[For more information, seeSub, Exit Sub, EndSub (Begin, Exit, End Subroutine).](../Instructions/subexitsubendsub.md)

## Program

See [BeginProg, EndProg (Begin Program, End Program)](../Instructions/beginprogendprog.md) for additional information.

** Set Scan Interval **: Set how often the measurements are made.[For more information, seeScan, NextScan (Set Scan Rate, Proceed to Next Scan).](../Instructions/scannextscan.md)

** Measurements **: Enter measurements to be made.

** Additional Processing **: Enter any additional processing that should be performed on the measurements.

** Call DataTables **: Call the defined DataTable(s) so that output data is processed.[For more information, seeCallTable (Call Table).](../Instructions/calltable.md)

** Initiate Controls **: Define any necessary controls.

** NextScan**: Loop back up for next scan.[For more information, seeScan, NextScan (Set Scan Rate, Proceed to Next Scan).](../Instructions/scannextscan.md)

End Program

## Line Continuation

Line continuation allows an instruction or logical line to span one or more physical lines. This allows you to break up long lines of code into more readable chunks . Line continuation is indicated by one white space character that immediately precedes a single underscore character as the last character of a line of text. Following is an example of line continuation:

Public Temp, RH, WindSp, WindDir, \_

BatteryV, IntRH, IntTemp, RainTot, \_

RainInt, Solar

A single line of code is limited to 512 characters (the buffer for a line of program code is 512 bytes). A "line of code" includes code that is placed on multiple lines using line continuation syntax.

## Multiple Instructions, Single Line

Multiple instructions can be placed on a single line by separating the instructions with a colon. As an example:

Public AirTemp

Units AirTemp = DegC

Can also be written as:

Public AirTemp : Units AirTemp = DegC

Note that with the single line form of an If/Then/Else statement, the implied EndIf occurs at the very end of the line; for example,

If A > 10 Then A = A + 1 : B = B + A : C = C + B {EndIf}

## File Names

In general, all file names used in CRBasic programming should be limited to a maximum of 59 characters. This includes names of CRBasic programs themselves.

## Line Limits

A single line of code is limited to 512 characters (the buffer for a line of program code is 512 bytes). A "line of code" includes code that is placed on multiple lines using line continuation syntax (space underscore " \_").

If A > 10 Then A = A + 1 {EndIf} : B = B + A : C = C + B

## CR200 Series Limitations

### Variables

The CR200 series dataloggers can accommodate up to 60 variables in a program (note that an array uses multiple variables; for instance, Temp(10) will use 10 variables). The CR200X is limited to 128 variables. If your program exceeds this limit, an error message will occur. This number includes variables declared in the program and variables used by the program for data table output. Output instructions may consume multiple variable locations, as follows:

- Average = 1 + reps

- Totalize = 1 + reps

- StdDev = 1 + 3 \* reps

- Maximum, without time = 1 + reps

- Maximum, with time = 1 + 2 \* reps

- Minimum, without time = 1 + reps

- Minimum, with time = 1 + 2 \* reps

- WindVector = 5

### Data Tables

The CR200 series datalogger can support up to six data tables. This includes the Public and Status tables; therefore, up to four output tables can be defined. The CR200X datalogger can accommodate up to ten tables, the Public and Status table, and eight output tables.
