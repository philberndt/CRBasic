# Histogram4D (4D Histogram)

The Histogram4D instruction is used to process input data as either a standard histogram (frequency distribution) or a weighted value histogram of up to four dimensions.

## Syntax

Histogram4D(BinSelect,DataType,DisableVar,Bins1,Bins2,Bins3,Bins4,Form,WtVal,Lolim1,UpLim1,Lolim2,UpLim2,Lolim3,UpLim3,Lolim4,UpLim4)

The following partial code provides an example for using the Histogram4D instruction.

```
'\\\\\\\\\\\\\\\\\\\\ VARIABLES and CONSTANTS ////////////////////
Public Panel_Temp
Public Volts
Public Temp
Dim Bin(2)
Units Bin = Percent
'\\\\\\\\\\\\\\\\\\\\\\\\ OUTPUT SECTION ////////////////////////
DataTable ("HIST4D",1,100)
DataInterval(0,1,Sec,100)
Histogram4D(Bin(), FP2, 0, 2, 4, 0, 0, 001, 100, 12, 14, -25, 0, 0, 0, 0, 0)
EndTable
DataTable ("VALUES",1,100)'Trigger, buffer of 100 Records
DataInterval(0,1,min,10)'Synchronous, 100 lapses
Average(1,Volts,FP2,0)'Reps,Source,Type
Average(1,Temp,FP2,0)'Reps,Source,Type
EndTable
'\\\\\\\\\\\\\\\\\\\\\\\\\\\ PROGRAM ///////////////////////////
BeginProg
Scan (1,Sec,0,0)
Battery(Volts)'main battery volts
PanelTemp (Panel_Temp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,Panel_Temp,True,0,15000,1.0,0)

Bin(1) = Volts
Bin(2) = Temp
CallTable VALUES
CallTable HIST4D
NextScan
EndProg
```

## Remarks

The Histogram4D instruction is similar to the [Histogram](histogram.md) instruction, except that the Histogram4D instruction allows up to four BinSelect inputs (four dimensions). The BinSelect values are specified as a variable array. Each of the BinSelect values has its own range and number of weighted values.

## Parameters

# BinSelect (Bin Select)

The variable that is tested to determine which bin is selected. This variable must be dimensioned to the same size as the number of dimensions used in the Histogram4D instruction - a number between 1 and 4.

Type: Variable

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

** NOTE:**If FP2 is selected, the largest number that can be accumulated in each bin before a numeric overrange occurs is 7999.

# DisableVar

A Constant, Variable, or Expression that is used to determine whether the current measurement is included in the output saved to the DataTable. Right-click the parameter to display a list.

- 0 = Process current measurement

- Non-zero = Do not process current measurement.

** NOTE:**For all instructions except Totalize, if the disable variable is true over the entire output interval,NAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

will be stored. For Totalize, if the disable variable is true over the entire output interval, 0 will be stored

[See alsoMultipliers, Offsets, and Disable Variables with Repetitions.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Expression

# Bins

Defines the number of bins or subranges to include in the histogram bin select range. The total number of bins (subranges) will be equal to the product of Bin1, Bin2, Bin3, Bin4. The width of each subrange is equal to the BinSelect range (UpLim - LoLim) divided by the number of bins.

If all four dimensions are used then the output will be linearized. Since the OS only keeps three dimensions internally, the output must be linearized to successfully pass the table definitions to datalogger support software.

- ** Bins1 **: The row-wise number of bins to include in the histogram.

- ** Bins2 **: The column-wise number of bins to include in the histogram.

- ** Bins3 **: The 3rd dimension number of bins to include in the histogram.

- ** Bins4 **: The 4th dimension number of bins to include in the histogram.

Type: Constant (or expression that evaluates as a constant)

# Bins

Defines the number of bins or subranges to include in the histogram bin select range. The total number of bins (subranges) will be equal to the product of Bin1, Bin2, Bin3, Bin4. The width of each subrange is equal to the BinSelect range (UpLim - LoLim) divided by the number of bins.

If all four dimensions are used then the output will be linearized. Since the OS only keeps three dimensions internally, the output must be linearized to successfully pass the table definitions to datalogger support software.

- ** Bins1 **: The row-wise number of bins to include in the histogram.

- ** Bins2 **: The column-wise number of bins to include in the histogram.

- ** Bins3 **: The 3rd dimension number of bins to include in the histogram.

- ** Bins4 **: The 4th dimension number of bins to include in the histogram.

Type: Constant (or expression that evaluates as a constant)

# Form

The Form argument consists of three elements: ABC. Open form includes values > UpLim in the upper bin and values < LoLim +NAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

s in the lower bin. Closed form excludes values outside of the limits (including NANs). Right-click within the parameter to display a list.

| Code  | Form                                                           |
| ----- | -------------------------------------------------------------- |
| A = 0 | Reset histogram values to 0 after each output.                 |
| A = 1 | Do not reset histogram.                                        |
| B = 0 | Divide bins by total count.                                    |
| B = 1 | Output total in each bin.                                      |
| C = 0 | Open form. Include values outside LoLim and UpLim in end bins. |
| C = 1 | Closed form. Exclude values outside range.                     |

Type: Constant

When a histogram is reset, a new histogram is created and stored in the data table at the data table's output interval. The number of new data points that are used for calculating subsequent histograms is determined by the ratio of the data table output rate to the scan interval: (data table output rate)/(scan interval). Choose whether or not the histogram should be reset (all bins are set to zero) after each output. If the histogram is not reset, the values in each bin will continue to accumulate as long as the program runs.

When element C is set to 0 (open form), NAN values accumulate in the first bin. If this is undesirable, these values can be filtered using programming logic (for example, "If variable = NAN then DisableVar = 1").

# WtVal (Weighted Value)

The variable name of the weighted value. Enter a constant for a frequency distribution of the BinSelect value. Right-click the parameter to display a list of defined variables.

Type: Constant or Variable

# LoLim1234 (Lower Limit)

The lower limit of the range covered by the BinSelect value

Type: Constant (or expression that evaluates as a constant)

# UpLim1234 (Upper Limit)

The upper limit of the range covered by the BinSelect value.

Type: Constant (or expression that evaluates as a constant)

Output:

For a 4Dim histogram with # of Bins in each dimension as follows:

|                                       |      |
| ------------------------------------- | ---- |
| # of Bins in First Dimension (Bins1)  | = B1 |
| # of Bins in Second Dimension (Bins2) | = B2 |
| # of Bins in Third Dimension (Bins3)  | = B3 |
| # of Bins in Fourth Dimension (Bins4) | = B4 |

The total number of bins is the product of the number of bins in each dimension (B1 x B2 x B3 x B4). The output would be arranged sequentially in the order:

[Bin(1,1,1,1), Bin(1,1,1,2), Bin(1,1,1,B4), Bin(1,1,2,1), Bin(1,1,2,2), ... Bin(1,1,2,B4), Bin(1,1,3,1), Bin(1,1,3,2) ... Bin(1,1,3,B4) ... Bin(1,1,B3,1), Bin(1,1,B3,2), ... Bin(1,1,B3,B4), Bin(1,2,1,1), Bin(1,2,1,2), ... Bin(1,2,1,B4), Bin(1,2,2,1), ... Bin(1,2,2,B4), Bin(1,2,3,1), Bin(1,2,3,2), ... Bin(1,2,B3,1), Bin(1,2,B3,2) ... Bin(1,2,B3,B4), Bin(1,3,1,1), Bin(1,3,1,2), ... Bin(1,B2,B3,B4), Bin(2,1,1,1), ... Bin(B1,B2,B3,B4).

So if B1 = B2 = B3 = B4 = 2 (2 Bins in each dimension) then the output order would be:

Bin(1,1,1,1), Bin(1,1,1,2), Bin(1,1,2,1), Bin(1,1,2,2), Bin(1,2,1,1), Bin(1,2,1,2), Bin(1,2,2,1), Bin(1,2,2,2), Bin(2,1,1,1), Bin(2,1,1,2), Bin(2,1,2,1), Bin(2,1,2,2), Bin(2,2,1,1), Bin(2,2,1,2), Bin(2,2,2,1), Bin(2,2,2,2)
