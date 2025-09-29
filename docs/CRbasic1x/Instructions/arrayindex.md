# ArrayIndex (Array Index)

The ArrayIndex function is used to return the index of a named element in an array.

## Syntax

ArrayIndex([Name](#Name))

In the following program example, the ArrayIndex function is used within the Sample instruction to store the current reading of two sensors. Fieldname is used after each sample to provide a unique output name to the values (if Fieldnames is omitted, a compiler error would be returned indicating duplicate output names).

```
Const NumValues = 12
Public CWSArray(NumValues)
Public BattV

DataTable (WSN5min,True,1000)
DataInterval (0,5,min,10)
Sample (1,CWSArray(ArrayIndex("CWS900_TS")),FP2)
FieldNames ("CWS900_Temp")
Sample (1,CWSArray(ArrayIndex("CWS655_VWC")),FP2)
FieldNames ("CWS655_VWC")
EndTable

BeginProg
Scan (5,min,3,0)

Battery (BattV)
```

CWB100 (C1,CWSArray )

```
CallTable (WSN5min)
NextScan
```

EndProg

```

## Remarks


The ArrayIndex function is used to return the index of a named element in an array, which would otherwise be unknown. The value can then be further processed in the program because of its known position in the array. If the named element is not found, the function returns 0 (this will result in a Variable Out of Bounds error).

One use of this instruction is in a wireless sensor network where auto-discovery is being used. In this instance, the sensor measurements are returned in the destination array in the order in which the sensors are discovered by the base. It is not known until the sensors are discovered where in the array each sensor's measurement values will be stored. The names of the values returned by the sensors are known, however. Thus, the ArrayIndex function can be used to return the correct index value so that further processing can be done (for example, output processing or unit conversion). This instruction can be used similarly with the [NewFieldNames](newfieldnames.md) instruction.


## Parameter



# Name


A string that contains the name of the value for which an index is desired. All sensors have a default sensor name, and a fieldname for each returned value. For instance, a CWS900 with a 109 probe attached returns four values Ts (temperature), Ti (internal temperature), BV (battery voltage), and SS (signal strength). Thus, the default name for the sensor's temperature measurement might be CWS900\_1\_Ts. This would be entered as a string in the ArrayIndex function. Unique sensor names and fieldnames can be assigned using Device Configuration Utility, Network Planner, or Wireless Sensor Planner.

Type: String
```
