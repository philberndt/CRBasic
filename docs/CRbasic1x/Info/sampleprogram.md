# Sample Programs

The following program provides an example of a typical CRBasic program. The program measures the datalogger's panel temperature, and uses that value as a reference for a thermocouple measurement.

The lines of text proceeded by a single quote are comments. They are ignored when the program is compiled. Comments can be used to explain what the program does and provide information on specific instructions.

**NOTE:** The help contains example program code for all instructions in the datalogger. Look for the Example link at the top of each instruction topic.

```
'CR1000X Series
'Program Author: Your Name
'Date: 3/15/19
'Description: Program to Measure a Differential Thermocouple
'Program Declarations

'Variables to be used in the Program
Public PanelT, TCTemp

'DataTable Definitions
'Create a data table named TempData that holds 1000 records
DataTable (TempData,1,1000)
'Set data storage interval to 1 minute
DataInterval (0,1,Min,10)
'Sample the value in the Thermocouple variable
Sample (1,TCTemp,FP2)
EndTable

'Main Program
BeginProg
'Set program scan interval to 1 second
Scan (1,Sec,3,0)
'Measure Panel Temperature; store result in variable PanelT
PanelTemp (PanelT,15000)
'Measure Thermocouple; store result in TCTemp
TCDiff (TCTemp,1,mv200C,1,TypeT,PanelT,True ,0,15000,1.0,0)
'Call the datatable
CallTable TempData
'Return to top of program; program executed at next scan interval
NextScan
EndProg
```

Note that even though the TempData data table is called every scan interval (every second) in the program, because the DataInterval is set to 1 minute, a sample value is stored every 60 scans (60 seconds) only.
