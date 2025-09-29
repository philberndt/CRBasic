# ReadOnly (Set Public Variables as Read Only)

The ReadOnly instruction is used to set one or more Public variables as read only.

## Syntax

ReadOnlyVar1, Var2, Var3

In the following program, two Public variables (Mult and Offset) are declared as read-only. They can be viewed but not edited.

```
Public IntTemp, TCTemp, Batt, Mult = 1.8, Offset = 32
ReadOnlyMult, Offset

BeginProg
Scan (1,Sec,3,0)
Battery (Batt)
PanelTemp (IntTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,IntTemp,True ,0,4000,Mult,Offset)

NextScan
EndProg
```

## Remarks

Variables declared as read only can be viewed using the software's numeric display, but they cannot be edited outside of the program (they are editable under program control).

More than one variable can be set to read only with a single ReadOnly instruction by separating the variables with commas, as shown in the earlier example.

When an [Alias](alias.md) is assigned to a singular Public variable (that is, not an array), the ReadOnly flag must be set on the Public variable, not the Alias. However, for a Public variable array, the ReadOnly flag must be set on the Alias(es), not the Public variable. This allows a Public variable array to have both ReadOnly and editable variables within the same array. For example:

> ```
> Public Array(3)
> Alias Array(1)=ReadOnlyAliasOne
> Alias Array(2)=ReadOnlyAliasTwo
> Alias Array(3)=EditableAliasThree
> ReadOnlyReadOnlyAliasOne,ReadOnlyAliasTwo
> ```
>
> ReadOnly applies toPakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

communication and variables requested usingModbus

**Note:** Communication protocol published by Modicon in 1979 for use in programmable logic controllers (PLCs).

communication.
