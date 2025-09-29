# BeginProg, EndProg (Begin Program, End Program)

The BeginProg instruction is used to mark the beginning of a program. EndProg marks the end of a program.

## Syntax

```
BeginProg
```

...

...

```
EndProg
```

The following code shows the layout of a typical datalogger program and the use of the BeginProg/EndProg statements. Program variables and the Data table are defined, followed by the code for the main program.

```
'Declare public variables
Public Ptemp, Batt_volt
```

'Define Data Tables
DataTable (Test,1,-1)'Set table size to # of records, or -1 to autoallocate
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Ptemp,FP2)
EndTable

```
'MainProgram
BeginProg'main program begins
Scan (1,Sec,0,0)'scan at 1 second intervals in an infinite loop
PanelTemp (PTemp,60)'read temperature of the dataloggerwiring panelin degrees Celsius
Battery(Batt_volt)'read the voltage of the battery powering the datalogger in volts
'Enter other measurement instructions
'Call Output Tables
CallTable Test
NextScan'scan again
```

EndProg'main program ends

```

## Remarks


All of the instructions for the main program fall between the BeginProg and EndProg statements. Program Variables, Data tables, and Subroutines must be defined before the main program.
```
