# AddPrecise (Add High Precision)

The AddPrecise function allows you to do high precision totalizing of variables or manipulation of high precision variables.

## Syntax

AddPrecise(PrecisionVariable,X)

This program demonstrates the difference in values when high precision addition is used and when normal precision addition is used.

The program starts with two variables set equal to .111111. One variable is normal precision and the other is high precision (by using the MovePrecise instruction). The program then adds .000000111111 one hundred thousand times to each value (this is equivalent to .111111 + 100000 \* .000000111111 or .111111 + .0111111 = .1222221). The variable Diff is used to track the difference in the two values each scan through the program.

At the end of the program, the variable Precise will equal .1222221 (precisely correct). The variable NotPrecise will equal .1222869 (not precisely correct).

```
Public Precise, NotPrecise, Diff

BeginProg
'Set variables to .111111
MovePrecise(Precise, .111111)
NotPrecise = .111111
Scan (10,mSec,3,100000)'scan 100,000 times
'Add Value to each variable
AddPrecise(Precise, .000000111111)
NotPrecise = NotPrecise + .000000111111
'Track difference in values
Diff=NotPrecise-Precise
NextScan
EndProg
```

## Remarks

In this function, the variable X is added to the PrecisionVariable. Every reference to the PrecisionVariable will cause a 32 bit extension of its mantissa to be saved and used internally. A normal single precision float has 24 bits of mantissa; therefore, this new precision is 56 bits. This function can be useful when trying to find the difference between two high precision variables.

## Parameters

# PrecisionVariable (Precision Variable)

The variable that will be affected by the precision move or add.

Type: Variable

# X (Value to Move or Add)

The value that will be moved to or added to the PrecisionVariable. It may or may not be a high precision variable, depending upon whether it has been declared as such in a previous AddPrecise or MovePrecise instruction.

Type: Variable

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [AddPrecise](#),[Average](average.md),[AvgRun](avgrun.md),[AvgSpa](avgspa.md),[CovSpa](covspa.md),[MinRun](minrun.md),[MaxRun](maxrun.md),[MovePrecise](moveprecise.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevRun](stddevrun.md),[StdDevSpa](stddevspa.md),[TotalRun](totalrun.md), and [Totalize](totalize.md).
