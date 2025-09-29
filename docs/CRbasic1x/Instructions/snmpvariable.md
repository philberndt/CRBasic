# SNMPVariable (Define Custom MIB Hierarchy for SNMP)

The SNMPVariable instruction is used to define a custom MIB (Management Information Base) hierarchy for SNMP.

## Syntax

SNMPVariable(Name,OID,Type[optional],Access[optional],Valid[optional] )

The following example program demonstrates SNMPVariable

```
'Declare Variables
Public LoggerTemp, BattV
Public MyTemp, ValFlt
Public ValInt as Long

DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,BattV,FP2,False,False)
Sample(1,LoggerTemp,FP2)
EndTable

BeginProg
ESSInitialize("private,public")
SNMPVariable(MyTemp,1.3.6.1.4.1.43592.55.1.4,Float,RONLY)
SNMPVariable(ValFlt,1.3.6.1.4.1.43592.55.1.5,Float,RWRITE)
SNMPVariable(ValInt,1.3.6.1.4.1.43592.55.1.6,Integer,RWRITE)
Scan (1,Sec,0,0)
MyTemp += 0.001
PanelTemp (LoggerTemp,15000)
Battery (BattV)
CallTable Test
NextScan
EndProg
```

## Remarks

The [ESSInitialize](essinitialize.md) instruction must be in the program to enable SNMP in the datalogger. Both ESSInitialize and SNMPVariable should be included in the program after the BeginProg statement.

## Parameters

# Name

The name of the variable or variable array in the CRBasic program. If the variable is not already declared, it will be declared by this instruction as a Public variable.

Type: Variable

# OID

The object identifier, represented using dot notation; e.g., 1.3.6.1.4.1.111.4.1.1. If the variable is an array, elements of that array are accessed using the syntax root.element (where root is the OID and element is the element number of the array; e.g., 1.3.6.1.4.1.111.4.1.1.2 specifies element number 2 of the object ID 1.3.6.1.4.1.111.4.1.1 variable).

In addition to the user defined OIDs, the following OIDs are supported in the datalogger:

- 1.3.6.1.2.1.1.1 sysDescr

- 1.3.6.1.2.1.1.2 sysObjectID

- 1.3.6.1.2.1.1.3 sysUpTime

- 1.3.6.1.2.1.1.4 sysContact

- 1.3.6.1.2.1.1.5 sysName

- 1.3.6.1.2.1.1.6 sysLocation

- 1.3.6.1.2.1.1.7 sysServices

When [ESSVariables](essvariables.md) and SNMPVariable are used in the same program, these user-defined OIDs must begin with an identifier of 1.3.6.1.4.1.1207 or greater (the last OID in the datalogger OS structure for ESSVariables is 1.3.6.1.4.1.1206).

Type: Constant

## Optional ParameterS

# Type

The data type of the managed object. Type can be Integer, Counter, String, TimeTicks, Opaque, Gauge, or Float. If this parameter is omitted, the object assumes the data type of the variable declared in the CRBasic program. If a variable has not been declared, the default is Integer. If the type is Float, the OID is advertised as a String and conversion takes place automatically (since SNMP does not natively support a type of Float).

Type: String

# Access

By default, an object is defined as read-only. RWRITE can be used in the Access parameter to set the object to read/write.

Type: String

# Valid

Used to define a valid range or set of values that can be written. RANGES is used to set a range of values; e.g., RANGES 0,65535 defines a range of values of 0 to 65535. VALUES 1,2,3,4,5 defines valid input as the numbers 1 2 3 4 and 5. If the Valid parameter is omitted, any value can be written.
