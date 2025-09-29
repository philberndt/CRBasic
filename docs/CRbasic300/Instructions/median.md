# Median (Save Median Value)

The Median instruction stores the median of a variable, over time, in an output table.

## Syntax

Median(Reps,Source,MaxN,DataType,DisableVar)

In the following program, the median is found among 60 values for each of three thermocouples. The result is stored in a one minute data table.

```
Public Temp(3), RefTemp

DataTable (TempTab,True,-1)
DataInterval (0,60,Sec,10)
Median(3,Temp(),60,FP2,False)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (Temp(),3,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable (TempTab)
NextScan
EndProg
```

## Remarks

Median is an output instruction that is included within a data table declaration. Each time the data table is called and the output condition is False, the current Source value is stored to an array in memory. This array is dimensioned with MaxN number of elements; therefore, no more than MaxN values are retained in memory. If the output condition is not set true prior to MaxN being reached, the oldest value stored in the array is discarded. When the output condition is True, the instruction returns the Median of those values and memory for the instruction is cleared. If the number of values for which the median is calculated is an even number, the two median values are averaged.

If the DisableVar is true for the entire output interval,NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is stored. Also, If a NAN is included in the values being processed, NAN is stored. Note that since there is no such thing as NAN for integers, values that are converted from float to integer are expressed in data tables as the most negative number for a given data type. For example, the most negative number of data typeFP2

**Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4.

is -7999, so NAN for FP2 data will appear in a data table as -7999. If the data type isLong

**Note:** Data type used when declaring a variable as an integer.

, NAN will appear in the data table as -2147483648.

Call the output table conditionally (for example, do not call the table if a variable = NAN) to keep NANs from affecting the other good values.

## Parameters

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the Median instruction, the Reps are the number of variables for which to calculate a median (a separate Median is calculated for each variable). If the Reps parameter is greater than 1, an array must be specified for Source. If not, a Variable Out of Bounds error is returned when the program is compiled.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

Source may also be anexpression. The resulting fieldname is AnonymousN, where N is an incrementing number assigned to each output value entered as an expression. The FieldNames instruction can then be used to change AnonymousN to a more appropriate fieldname; for example,

Sample (1,PTemp\* 1.8+32,FP2)

FieldNames("PTempDegFSample:Sample Panel Temp in degrees F")

# MaxN (Maximum Number)

The maximum number of values the datalogger should maintain in memory for the instruction, from which the median will be calculated. This memory is set up as ring memory, meaning old values will be overwritten with new values if the number of values stored before output is greater than MaxN. Thus, the last MaxN values will be used to calculate the output.

Type: Constant

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
