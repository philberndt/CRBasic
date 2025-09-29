# MaxSpa (Maximum Spatial)

The MaxSpa function is used to find the maximum value in an array.

## Syntax

MaxSpa(Dest,Swath,Source)

This example uses MaxSpa to find the maximum value of three elements, Temp(1) through Temp(3). The spatial maximum is stored in MaxTemp(1) and the element number of the value is stored in MaxTemp(2)

```
'CR350Datalogger

Public Temp(3), TRef, MaxTemp(2)

BeginProg
Scan( 100,mSec,1,0 )
PanelTemp (Tref,4000)
TCDiff (Temp(),3,mv34,1,TypeT,Tref,True,0,4000,1.8,32)

MaxSpa(MaxTemp(),3,Temp())
NextScan
EndProg
```

## Remarks

The MaxSpa function finds the maximum value and its location in the given Source array and places the results in the variable array named in Dest. Source and/or Dest can be aFloat

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

orLong

**Note:** Data type used when declaring a variable as an integer.

data type, but not aString

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

.

If aNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is returned by the datalogger it is not included in the spatial maximum.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the MaxSpa instruction, the Dest variable must be dimensioned to 2. Dest(1) will contain the value and Dest(2) will contain the location.

# Swath

The number of values of the array over which to perform the specified operation.

Type: Constant (or expression that evaluates as a constant)

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable
