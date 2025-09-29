# RMSSpa (Spatial Root Mean Square)

The RMSSpa function is used to compute the root mean square (RMS) value of an array. This instruction, when used, must appear between the Scan NextScan instructions.

## Syntax

RMSSpa(Dest,Swath,Source)

This example uses RmsSpa to find running root mean square value (RMSVolts) of 50 elements in the SampledACVolts(50) array. The array fills from a small AC voltage applied to differential terminal 1.

```
'CR300
Public RMSVolts
Dim I
Dim SampledACVolts(50)

DataTable (RMS,1,1000)
DataInterval (0,1,sec,10)
Sample (1,SampledACVolts(),FP2)
Sample (1,RMSVolts,FP2)
EndTable

BeginProg
Scan(25,mSec,1,0)
I=I+1
VoltDiff(SampledACVolts(I),1,mv2500,1,1,0,4000,1,0)

If I>49
i=0
endif
CallTable RMS
'Put the RMS of SampledACVolts()in RMSVolts
RmsSpa(RMSVolts,50,SampledACVolts(1))
NextScan
EndProg
```

## Remarks

The RMSSpa function is defined as:

Where X(i) = Source

If a NAN is returned by the datalogger it is not included in the spatial RMS.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Swath

The number of values of the array over which to perform the specified operation.

Type: Constant (or expression that evaluates as a constant)

For the RMSSpa instruction, the Swath parameter is the number of elements to include in the calculation.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [Average](average.md),[AvgRun](avgrun.md),[AvgSpa](#),[CovSpa](covspa.md),[RMSSpa](#),[StdDev](stddev.md),[StdDevSpa](stddevspa.md), and [Totalize](totalize.md).
