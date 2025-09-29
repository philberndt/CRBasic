# Tablename.Tablebytes

The TableName.TableBytes syntax is used to return the number of bytes that have been allocated in the program for a DataTable. This fieldname is only available in the CR6, CR1000X, and GRANITE Series dataloggers.

## Syntax

TableName.TableBytes

This example program uses the TableName.TableBytes syntax to return the size in bytes of data tables stored on a CRD: and on the CPU:

```
'Format CRD before running this program to get expected TableBytes

'Declare Variables
Public Count as LONG
Public PTemp, Batt_volt,Lastfilename as string * 25,outstat as Boolean
Public Tbytes(6) as LONG

'Define Data Tables
'Auto allocate CPU
DataTable (Test1,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Count,Long)
Sample (1,PTemp,FP2)
EndTable

'DataInterval
DataTable (Test2,1,10)
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Count,Long)
Sample (1,PTemp,FP2)
EndTable

'No DataInterval
DataTable (Test3,1,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Count,Long)
Sample (1,PTemp,FP2)
EndTable

'Tablefile
DataTable (Test4,1,-1)
Tablefile ("CRD:TableName",64,-1,0,24,Hr,outstat,Lastfilename)
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Count,Long)
Sample (1,PTemp,FP2)
EndTable

'CardOut
DataTable (Test5,1,-1)
CardOut (0 ,1000)
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Count,Long)
Sample (1,PTemp,FP2)
EndTable

'DataEvent
DataTable (Test6,1,10)
DataEvent (0,PTemp>35,PTemp<34,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,Count,Long)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,1)
Count+=1
PanelTemp (PTemp,15000)
Battery (Batt_volt)
Tbytes(1) =Test1.TableBytes
Tbytes(2) =Test2.TableBytes
Tbytes(3) =Test3.TableBytes
Tbytes(4) =Test4.TableBytes
Tbytes(5) =Test5.TableBytes
Tbytes(6) =Test6.TableBytes
CallTable Test1
CallTable Test2
CallTable Test3
CallTable Test4
CallTable Test5
CallTable Test6
NextScan
EndProg
```

## Remarks

The name of the DataTable is entered in place of the TableName parameter. TableName is limited to 20 characters. The size, in bytes, of the DataTable defined by the TableName parameter is returned.

For additional data table access functionality, see [Data Table Access](../Info/datatableaccess.md).
