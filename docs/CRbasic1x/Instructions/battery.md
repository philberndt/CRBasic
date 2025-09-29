# Battery

The Battery instruction is used to readthe battery voltage.

## Syntax

```
Battery
```

(Dest)

The following program makes a simple battery measurement in the datalogger and stores a sample to a data table.

```
Public Batt_volt

DataTable (Table1,True,-1)
DataInterval (0,1,Min,10)
Sample (1,Batt_volt,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery(Batt_volt)
CallTable (Table1)
NextScan
EndProg
```

## Remarks

This instruction reads the voltage of the battery powering the system and stores the result in the Dest variable. The units for battery voltage are volts.

**NOTE:** The datalogger performs the same battery measurement whether it is a measurement performed in the background or as a result of the Battery instruction in the program (some otherCampbell Scientificdataloggers perform a different measurement, based on whether it is a background measurement or the Battery instruction).

**Caution:** Do NOT place this instruction inside a conditional statement when running inpipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.
