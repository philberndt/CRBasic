# WorstCase (Worst Case Data Storage Events)

The WorstCase instruction is used within a program to save one or more "worst case" data storage events into separate tables.

## Syntax

WorstCase(TableName,NumCases,MaxMin,Change,RankVar)

This program demonstrates the Worst Case Instruction. The trigger for the start of a data event is when TC(1) exceeds 30 degrees C. To use the worst case instruction with events of varying duration, the event table size must be selected to accommodate the maximum duration expected (OR needed). The ranking criteria is the Max temperature that TC(1) sees during the triggered event. The greater the temperature the "worse" the event.

```
'Declare Variables
Const RevDiff=1 : Const Del=100 : Const Integ=15000: Const NumCases=5 : Const Max=1
Public RefTemp, TC(5) : Units RefTemp=degC : Units TC=degC
Public I, MaxTemp'Declare index and the ranking variable

DataTable (Evnt,1,10)
DataInterval(0,0,msec,10)'Set the sample interval equal to the scan
DataEvent(1,TC(1)>30,-1,8)'1 records before TC(1)>30, 8 records after TC(1)>30
Sample(1,RefTemp,IEEE4)'Sample the reference temperature
Sample(5,TC,IEEE4)'Sample the 5 thermocouple temperatures
EndTable

BeginProg
Scan(500,mSec,0,0)
PanelTemp (RefTemp,15000)
TCDiff (TC,5,mv200C,1,TypeT,RefTemp,RevDiff,Del,15000,1.0,0)

CallTable Evnt
If Evnt.EventEnd(1,1)'Check if an Event just Ended
MaxTemp = 0'Initialize MaxTemp below lowest threshold possible
For I = 1 To 10'Loop through TC measurements to find event max
If Evnt.TC(1,I) > MaxTemp Then MaxTemp = Evnt.TC(1,I)
Next I
WorstCase(Evnt,NumCases,Max,0,MaxTemp)'Check for worst case
EndIf
NextScan
EndProg
```

## Remarks

To use WorstCase, a DataTable sized to hold one data storage event must be created to act as the event buffer. Data can be stored to the table using the DataEvent instruction or some other trigger condition.

The user must create an algorithm in the program by which to test the WorstCase event. The algorithm should calculate a numerical ranking value of the event, which is stored as a variable.

When WorstCase is executed, it checks the ranking variable. When checking for Max Worst Cases (MaxMin option set to 1), if the current value of the ranking variable has a higher value than the lowest ranked WorstCase DataTable clone's recorded ranking variable, then the new data in the event DataTable will replace the data in this clone.

When checking for Min Worst Cases (MaxMin option set to 0), if the current value of the ranking variable has a lower value than the highest ranked WorstCase DataTable clone's recorded ranking variable, then the new data in the event DataTable will replace the data in this clone.

Multiple WorstCase events can be saved. The number of WorstCase events is specified with the NumCases variable. A separate table is created for each of the WorstCase events. These tables use the name of the DataTable with a two-digit number appended to the end (i.e., a table called Temp's WorstCase tables will be named Temp01, Temp02, Temp03 ). An additional table, nameWC (e.g., TempWC), stores the rank variable values for each of the worst case tables as well as the last time to which each table was written.

WorstCase must be used with data tables sent to the CPU. It will not work if the event table is sent to the PC card (CardOut).

The same data will not be written to two WorstCase Tables. If a trigger has occurred without the requisite number of pre-trigger records since the last event, the DataTable will not have the specified number of records.

If the data storage event table is stored on a memory card all the worst case tables will be stored on the same card. Because the WorstCase instruction requires the capability of erasing and writing over data, CPU Flash memory cannot be used with WorstCase.

## Parameters

# TableName

The TableName parameter is the name of the DataTable for which to create WorstCase Events. The name should be 4 characters or less so that the complete names of the worst case tables are retained when collected.

Type: Name

# NumCases

The number of worst cases" to store. A separate table is created for each worst case. The tables use the name of the DataTable with a two-digit number appended to the end (i.e., a table called Temp's WorstCase tables will be named Temp01, Temp02, Temp03 ). The numbers give the tables unique names, they have no relationship to the ranking of the events.

Type: Constant

# MaxMin

A constant that is entered to specify whether the maximum or minimum events should be saved. Right-click to display a drop-down list box.

| Value | Result                                                                                                                                                       |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0     | Min, save the events associated with the minimum rank variable; i.e., copy if Trigger is less than previous maximum (over event with previous maximum).      |
| 1     | Max, save the events associated with the maximum ranking variable; i.e., copy if Trigger is greater than previous lowest (over event with previous minimum). |

Type: Constant

# Change

The minimum change that must occur in the RankVariable before a new worst case is stored.

Type: Constant (or expression that evaluates as a constant)

# RankVar

The Variable by which to rank the events. Right-click to display a drop-down list box of defined variables.

Type: Variable
