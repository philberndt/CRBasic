# NewFieldNames (Change Field Name)

The NewFieldsNames instruction is used to change the fieldname of one or more generic variables in a data table during run-time.

## Syntax

```
NewFieldNames
```

(GenericName,NewNames)

The following program can be run to see the effects of the NewFieldNames instruction on the fieldnames of a data table.

To test:

1. Load the program then use software numeric display or a Keyboard Display to set the Flag variable to True. Setting the Flag variable to True forces the datalogger to update its table definitions.
2. After table definitions are updated, Fieldnames are changed from GenericName to SoilTemp, AirTemp, and WaterTemp.
3. You may need to click Start on Numeric Monitor to see the updated names.

```
Public GenericName(3), TRef
Public Flag As Boolean

DataTable (NewNamesTable,True,-1)
Sample (3,GenericName,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (Tref,15000)
TCDiff (GenericName(),3,mv200C,1,TypeT,TRef,True,0,15000,1.0,0)

If Flag
NewFieldNames(GenericName,"SoilTemp,AirTemp,WaterTemp")
Flag = False
EndIf
CallTable (NewNamesTable)

NextScan
EndProg
```

## Remarks

When using the NewFieldNames instruction, a variable array is given a generic name. Whenever the NewFieldNames instruction is executed, the next available generic variable in a data table is assigned a new name from the NewNames string.

This instruction accommodates smart sensors that return a unique name as part of a data string (where the unique name can be parsed out of the string and used for the NewName) or the addition of aCampbell Scientificwireless sensor into an existing wireless sensor network.

**NOTE:** When a NewName is assigned to a generic variable, the table definitions in the datalogger will change. Thus, any operation that relies on datalogger table definitions is affected. For example, if scheduled data collection is taking place, when the generic variable name is changed a backup file is created for the existing \*.dat file and a new file, with the new header information, is written.

Only one NewFieldNames instruction is allowed in a program.

# GenericName (Generic Name)

The name for the variable array assigned to the generic variable(s).

Type: Variable array

# NewNames (New Names)

A string that will be used to populate the generic variable field names when the NewFieldNames instruction is run. Multiple names in a list should be separated with commas.

Type: String (list of comma-separated names)
