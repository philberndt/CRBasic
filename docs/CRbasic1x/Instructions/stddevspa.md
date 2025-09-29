# StdDevSpa (Spatial Standard Deviation)

The StdDevSpa function is used to find the standard deviation of an array. This instruction, when used, must appear between the Scan NextScan instructions.

## Syntax

StdDevSpa(Dest,Swath,Source)

This example uses StdDevSpa to find the spatial standard deviation value of five samples from differential channel 1 and stores the result in the variable StdVolts.

```
Public StdVolts
Public SampledVolts(5)
Dim I
DataTable (MAIN,1,1000)
DataInterval (0,1,sec,10)'Store data every second
Average (1,StdVolts,FP2,0)
EndTable

BeginProg
Scan(100,msec,1,0)'Scan every 100 mSec
I=I+1'Sample 5 times each scan
VoltDiff(SampledVolts(I),1,mv5000C,1,1,200,15000,1,0)

If I>4
i=0
Endif
'Put the StdDev of SampledVolts() in StdVolts
StdDevSpa(StdVolts,5,SampledVolts())
CallTable MAIN
NextScan'Loop up for the next scan
EndProg
```

## Remarks

The spatial standard deviation is calculated as:

Where X(j) = Source

If a NAN is returned by the datalogger it is not included in the spatial standard deviation.

Source and/or Dest can be a Float or Long data type, but not a String.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Swath

The number of values of the array over which to perform the specified operation.

Type: Constant (or expression that evaluates as a constant)

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [Average](average.md),[AvgRun](avgrun.md),[AvgSpa](#),[CovSpa](covspa.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevSpa](#), and [Totalize](totalize.md).
