# AvgSpa (Spatial Average)

The AvgSpa function computes the spatial average of a measurement.

## Syntax

AvgSpa(Dest,Swath,Source)

This example uses AvgSpa to find the average value of three variables and store the result in the variable AvgVolts.

```
Public HiVolts(3), AvgVolts

BeginProg

Scan(1,Sec,1,0)
VoltDiff(HiVolts(),3,mV2500,1,0,0,4000,1,0)
AvgSpa(AvgVolts,3,HiVolts())
NextScan

EndProg
```

'Specify the specific array element where you want the sort to begin

```
Const NUM\_ROWS = 3
Const NUM\_COLUMNS = 4


```

'initialize a multi-dimensional array with test values

```
Public Test2DArray(NUM\_ROWS,NUM\_COLUMNS) = { 1, 2, 3, 4, 10, 12, 14, 16, 5, 10, 15, 20 }
Public TestAvgFloats(NUM\_ROWS)
Public TestStdFloats(NUM\_ROWS)

BeginProg

Dim i

Scan(1,Sec,0,0)
For i = 1 To NUM\_ROWS
AvgSpa (TestAvgFloats(i), NUM\_COLUMNS, Test2DArray(i,1))
StdDevSpa(TestStdFloats(i), NUM\_COLUMNS, Test2DArray(i,1))
Next i

NextScan

EndProg


## Remarks


The AvgSpa is calculated as:

Where X(j) = Source

If aNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is returned by the datalogger it is not included in the spatial average.

Source and/or Dest can be aFloat

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

orLong

**Note:** Data type used when declaring a variable as an integer.

data type, but not aString

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

.


## Parameters



# Dest (Destination)


The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array


# Swath


The number of values of the array over which to perform the specified operation.

Type: Constant (or expression that evaluates as a constant)

For the AvgSpa function, the Swath parameter is the number of elements to include in the average.


# Source


The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

For the AvgSpa function, the Source is the first variable in the array for which the spatial average should be calculated.

In the case of a multidimensional array as the Source, specify the array element where you want to start. For example, if the Source is a (4,3) array and you want to begin at element (3,1), enter Source(3,1).

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [Average](average.md),[AvgRun](avgrun.md),[AvgSpa](#),[CovSpa](covspa.md),[MinRun](minrun.md),[MaxRun](maxrun.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevRun](stddevrun.md),[StdDevSpa](stddevspa.md),[TotalRun](totalrun.md), and [Totalize](totalize.md).
```
