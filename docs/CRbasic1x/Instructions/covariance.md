# Covariance

The Covariance instruction calculates the covariance of elements in an array over time. This is a special instruction for use with Flux systems.

## Syntax

Covariance(DimX,XVal,DataType,DisableVar;NumOfCov)

The following is partial code that shows the use of the Covariance instruction.

```
Public aligned_data(6)
Alias aligned_data(1) = Uz
Alias aligned_data(2) = Ux
Alias aligned_data(3) = Uy
Alias aligned_data(4) = co2
Alias aligned_data(5) = h2o
Alias aligned_data(6) = Ts

DataTable (comp_cov,TRUE,1)
DataInterval (0,30,min,1)
Covariance(6,aligned_data(1),IEEE4,FALSE,21)
EndTable
```

## Remarks

The Covariance of X and Y is calculated as:

where _n_ is equal to the DataTable interval divided by the program execution interval (the number of measurements taken between each time the table is called)._Xi _ and*Yi * refer to the source array element number.

## Parameters

# DimX (Covariance - Number of Elements)

The number of elements in the source array for which covariance relationships will be calculated.

Type: Constant (or expression that evaluates as a constant)

# XVal (Covariance - First Element in Array)

The first element of the source array to be included in the covariance calculations. The X source must be an array that is dimensioned to at least the value of DIMX. Right-click the parameter to display a list of defined variables.

Type: Array Element

# DataType

A code to select the data storage format. Right-click the parameter to display a list.

| Name  | Description                                                                                        |
| ----- | -------------------------------------------------------------------------------------------------- |
| IEEE4 | IEEE four-byte floating point (IEEE 754 std). IEEE4 can also be specified using the keyword FLOAT. |
| FP2   | Campbell Scientifictwo-byte floating point.                                                        |
| IEEE8 | IEEE eight-byte floating point (double)                                                            |

Additional data types are available. However, not all data types are suitable for all output instructions, so they should be used with care.

| Name    | Description                                                        |
| ------- | ------------------------------------------------------------------ |
| String  | ASCII string; size defined by program                              |
| Boolean | 0 = False; -1 = True                                               |
| BOOL8   | 1-byte Boolean value                                               |
| Long    | 32-bit long integer, ranging from -2,147,483,648 to +2,147,483,647 |
| NSEC    | 8-byte time stamp format                                           |
| UINT1   | One-byte unsigned integer                                          |
| UINT2   | Two-byte unsigned integer                                          |
| UINT4   | Four-byte unsigned integer                                         |

**UINT1 **: Holds an 8-bit unsigned integer (a number in the range of 0 - 32,767, where NAN values are stored as 0). The program should be written to check for values outside the range. This data type is an efficient means of storing 8-bit integers since it requires only one byte of memory in a data table. Floating point values are converted to UINT1 as if the INT function were used.

** UINT2 **: Holds a 16-bit unsigned integer (a number in the range of 0 - 65535, where NAN values are stored as 0). The program should be written to check for values outside the range. This data type is an efficient means of storing 16-bit integers since it requires only two bytes of memory in a data table. Floating point values are converted to UINT2 as if the INT function were used.

** UINT4 **: Holds a 32-bit unsigned integer (a number in the range of 0 to 4,294,967,295).

** Long **: Sets the output to a 32-bit long integer, ranging from -2,147,483,648 to +2,147,483,647 (31 bits plus the sign bit). There are two possible reasons a user would use this format: (1) speed, since the OS can do math on integers faster than with floats, and (2) resolution, since the Long has 31 bits compared to the 24-bits in the IEEE4. However, in most instances it is not suitable for output since any fractional portion of the value is lost.

** BOOL8 **: Used to store variables that hold bits (0 or 1) of information. These values are shown in LoggerNet or other datalogger software as an array of eight Boolean values (each element in the array represents one bit). This data type uses less space than normal 32-bit values. Any reps stored must be divisible by two, since an odd number of bytes cannot be stored in a data table. When converting from a Long or a Float to a BOOL8, only the least significant 8 bits are used.

** NSEC **: An 8-byte time stamp format (4 bytes of seconds since 1990, 4 bytes of nanoseconds into the second) used when the variable being sampled is the result of the [RealTime](realtime.md) instruction or when the variable is a Long storing time since 1990 (for instance, when using TableName.TimeStamp). If the variable array is dimensioned to 7, the values stored will be year, month, day of year, hour, minutes, seconds, and milliseconds. The variable array must be declared as a Float or Long. If the variable array is dimensioned to two and declared as a Long, the instruction assumes that the first element holds seconds since 1990 and the second element holds microseconds into the second. If the source is a single variable dimensioned as a Long, the instruction assumes that the variable holds seconds since 1990 and the microseconds into the second is 0. In this instance, the value stored is a standard datalogger timestamp rather than the number of seconds since January 1990.

Type: Constant (or expression that evaluates as a constant)

# DisableVar

A Constant, Variable, or Expression that is used to determine whether the current measurement is included in the output saved to the DataTable. Right-click the parameter to display a list.

- 0 = Process current measurement

- Non-zero = Do not process current measurement.

** NOTE:**For all instructions except Totalize, if the disable variable is true over the entire output interval,NAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

will be stored. For Totalize, if the disable variable is true over the entire output interval, 0 will be stored

[See alsoMultipliers, Offsets, and Disable Variables with Repetitions.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Expression

For [Average](average.md),[Covariance](#),[Maximum](maximum.md),[Minimum](minimum.md),[Moment](moment.md),[StdDev](stddev.md),[Totalize](totalize.md)- if DisableVar is an array and Reps are greater than 1, a different DisableVar can be used for each rep.This follows the use of the input source such that Disable(i) would apply to Cov(Xi,Xj) for every j.

# NumOfCov

The number of Covariances to be computed. The maximum number is (DimX/2)\*(DimX+1).

If The Covariance instruction is Covariance(3,X(1),FP2,False,6) then there will be 6 covariance relationships calculated and sent to the output table:

Cov(X(1),X(1)), Cov(X(1),X(2)), Cov(X(1),X(3)), Cov(X(2),X(2)), Cov(X(2),X(3)), Cov(X(3),X(3))

Type: Constant (or expression that evaluates as a constant)
