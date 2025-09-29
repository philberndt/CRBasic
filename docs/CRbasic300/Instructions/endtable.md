# EndTable (End Data Table)

The EndTable statement is used to mark the end of a [DataTable](datatable.md) declaration.

## Syntax

EndTable

The following is a simple program measuring battery voltage and panel temperature. The DataTable/EndTable instructions are used to set up a table named Test.

```
Public PTemp, BattVolt

DataTable( Test,True,-1 )
DataInterval( 0, 1,Min,10 )
Sample (1,BattVolt,FP2)
Sample (1,PTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (BattVolt)
PanelTemp (PTemp,4000)
CallTable Test
NextScan
EndProg
```

## Remarks

For complete details on DataTable declarations, see [DataTable/EndTable (Define Output Table)](datatable.md).
