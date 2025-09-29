# Include

The Include instruction is used to include a portion of a program file that is not contained in the original program.

## Syntax

Include"Device:Filename"

Following is an example of using the Include file functionality of the datalogger. In the example, the "included" file merely declares a new variable and converts a temperature value in the original program to degrees Fahrenheit, but the included file could be a subroutine, slow sequence scan, or any portion of code that you did not want displayed in the main program.

Original program file

```
Public BattVolt, Temp

BeginProg
Scan (1,Sec,3,0)
Battery (BattVolt)
PanelTemp (Temp,4000)
Include"CPU:IncludeFile.cr300"
NextScan
EndProg
```

Include file

```
'this program must be named IncludeFile.cr300and stored on the datalogger's CPU drive

Public TempF

TempF=Temp*1.8+32
```

## Remarks

The Include file can be a subroutine, slow sequence, or any portion of code that you do not want to display in the main program. The code from the Include file is inserted in the program wherever the Include statement resides. If the Include file is not found on the datalogger (or in the same directory in which the file is being precompiled in CRBasic) an error message is returned. To see Include files expanded in a CRBasic program, use the **Conditional Compile, Include Files and Save** option from the [Compile menu](../Info/compilemenu.md). This compile option is available in LoggerNet version 4.5 and later.

## Parameter

# "Device: FileName"

The "Device:Filename" argument is the file that contains the additional code that should be executed. The Device on which the file is stored must be specified and the entire string must be enclosed in quotation marks.The only device choice for this datalogger is CPU.

The Include filename can also be an expression, such as "CPU:" + StationNameSetting + ".cr300", where StationNameSetting inserts the station name of the datalogger.
