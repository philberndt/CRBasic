# Units (Identify Measurement Units)

The Units argument is used to assign a label to a variable in the program to easily identify the units in which the associated measurement is stored.

## Syntax

Units`VariableA = UnitLabel`'assigns UnitLabel to VariableA

Units`VariableB() = UnitLabel`'assigns the same UnitLabel to all elements in the array

```
'To assign different units to variables in an array, they must first be Aliased:
```

Alias`VariableC(1) = MyVar :`Units`MyVar = Label1`

Alias`VariableC(2) = YourVar :`Units`YourVar = Label2`

Alias`VariableC(3) = OurVar :`Units`OurVar = Label3`

In the following example program, the Units instruction is used to define the unit of measure for the battery voltage and rainfall measurements. The units will show up in the header of the collected data file.

```
'Declare Variables and Units
Public Batt_Volt
Public Rain_in

UnitsBatt_Volt=Volts
UnitsRain_in=inch

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Totalize(1,Rain_in,FP2,False)
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'Default Datalogger Battery Voltage measurement Batt_Volt:
Battery(Batt_Volt)
'CS700 Rain Gauge measurement Rain_in:
PulseCount(Rain_in,1,P_LL,1,0,0.01,0)
'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

The Units declaration can reference Public, Dim, Alias, or FieldNames declarations.

This argument assigns a label only. It does not affect the actual value stored in the variable. The label is used in the header information of data tables, as well as the numeric monitor in software (if the Show Units option is available and enabled).

To assign the same units for an entire array, simply assign the units to the first element of the array. To assign different Unit declarations to each element in an array, first Alias the elements and then assign units to each.

| Unicode characters and ASCII character codes can be used for unit declarations. Unicode characters can be entered using the Tools | Insert Character menu item. ASCII character codes can be entered using the CHR() instruction. Note that a double quote (ASCII character code 34) cannot be used for a unit declaration since in CRBasic it denotes a string (though ASCII character code 148 closing quote can be used). |

The Units argument has these parameters:

# VariableA

The variable for which units will be assigned.

# UnitLabel

The label that should be used for units in the data tables. The maximum number of characters for this parameter is 38.
