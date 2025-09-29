# Alias

The Alias declaration is used to assign a second name to a Dim or Public variable.

## Syntax

AliasVariableName = AliasName

The example shows how to use the Alias declaration. TCTemp is the original variable array. CoolantT, ManifoldT, and ExhaustT are the aliases that are assigned to these variables.

```
Public TCTemp( 3 ), Ptemp
AliasTCTemp( 1 ) = CoolantT
AliasTCTemp( 2 ) = ManifoldT
AliasTCTemp( 3 ) = ExhaustT

BeginProg
Scan (1,Sec,1,0)
PanelTemp (Ptemp,60)

'Test for Alias instruction

TCDiff (TCTemp,3,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)
NextScan
EndProg
```

## Remarks

Both the original variable name and the Alias assignment can be used within the datalogger program. If the variable is declared as a Public variable, the Alias is used in the Public table. The Alias is also used as the root name for data table field names.

The VariableName argument is the name of the originally defined variable. The AliasName argument is the second name to be assigned to that variable. The maximum character length for aliases is 39. However, when outputting the Alias to a data table, the suffix containing the output type (for example, \_avg) is appended to the end of the variable name (Sample outputs have no suffix). Therefore, to stay within the 39 character limit, most Aliases should be no more than 35 characters (which allows for the 4 additional characters that may be needed for output processing identifiers).

The use of Aliases gives the programmer the efficiency of using arrays for measurement and processing, yet allows individually named measurements.

A swath of data can be Aliased by assigning a dimension to the AliasName (Alias VariableName(1) = AliasName(5) ). If the variable being aliased is an array, multiple aliases can be assigned on one line (Public Array(7) : Alias Array = FrontRoom, BedRoom, GreatRoom(4), Laundry).

**NOTE:** The alias or the variable name can be used within the datalogger program. However, if another device sends a request for a variable, the alias must be used in that request.
