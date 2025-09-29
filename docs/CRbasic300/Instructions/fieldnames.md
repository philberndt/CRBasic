# FieldNames (Field Names)

The FieldNames instruction is used immediately following an output processing instruction to change the default field names that the datalogger generates for results sent to the DataTable.

## Syntax

```
FieldNames
```

("Fieldname1:Description1,Fieldname2:Description2 " )

In the following program, the FieldNames instruction is used to assign individual names for three thermocouple measurements. These names are reflected in the output table instead of the array names.

```
Public PTemp, TCTemp(3)

DataTable (Test,True,-1)
Sample (3,TCTemp(),FP2)
FieldNames("AirTemp:, WaterTemp:, SoilTemp:")
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,3,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

CallTable Test
NextScan
EndProg
```

## Remarks

The FieldNames instruction must be placed inside the DataTable declaration immediately following the output instruction for which names are being created. The names are entered in the form of "Fieldname:Description". The fieldname and description must be separated by a colon, and the entire string must be enclosed in quotation marks.

As an example, the processing description and fieldname description for a Sample instruction might look like the following:

"TempSmp:This is a sample air temp"

If an output instruction generates multiple fields, individual names may be entered for each or a constant string with an index parameter indicating the number of times the string should be repeated may be used (for example, "NewNames(3)"). Individual names should be separated by a comma. If a String with an index is used, the name and reps must be specified (i.e., "NewNames(4)" specifies four field names; NewNames(1) through NewNames(4)). When the program is compiled, the datalogger will determine how many fields are created. If the list of names is greater than the number of fields, the extra names are ignored. If the number of fields is greater than the number names in the list of field names, the default names are used for the remaining fields.

## Parameters

# Fieldname (Field Name)

The name to be used for the value being output. It is a string that can be entered directly or represented by a constant. Field names are limited to 39 characters and cannot contain spaces.

If an output instruction generates multiple fields, individual names may be entered for each or a constant string with an index parameter indicating the number of times the string should be repeated may be used.

Type: Constant

# Description

Provides a way for the user to include further information about the field; this element is optional. In a collected data file, the Description is included in the header file below the Fieldname, along with the processing description.The maximum number of characters for the Description is 64, including quotation marks, spaces, and other characters.

Type: Text, including spaces
