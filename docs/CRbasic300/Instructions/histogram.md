# Histogram

The Histogram instruction is used to process input data as either a standard histogram (frequency distribution) or a weighted value histogram.

## Syntax

Histogram(BinSelect,DataType,DisableVar,Bins,Form,WtVal,Lolim,UpLim)

The following section of code demonstrates the use of the Histogram instruction.

```
Public Tblock(2),BattV,PTemp

DataTable (ACCEL,1,10000)'Trigger, buffer of ,10000 Records
DataInterval(0,1,Sec,100)'Synchronous, 100 lapses
'-------------------- Thermocouple Blocks --------------------
Sample (1,TBlock(),FP2)'Reps,Source,Type
'Histogram (src,type,disable,numbins,form,1,lolim,hilim)
Histogram( Tblock(), FP2, 0, 2, 001, 100, 10, 30 )
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (BattV)
PanelTemp (PTemp,4000)
TCDiff (TBlock(),2,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

CallTable ACCEl
NextScan
EndProg
```

## Remarks

The result of the Histogram instruction is a representation of a frequency distribution by using bins that have unique ranges. The value that is output to each bin is proportional to the frequency that the BinSelect's value is within that bin's range.

The individual Bin ranges are calculated by taking the difference between the upper and lower limits (UpLim and LoLim) and dividing by the number of Bins. The first bin's range starts at the lower limit. For example, if LoLim = 100, UpLim = 200, and Bins = 4, the bins are defined as:

|      |                     |
| ---- | ------------------- |
| Bin1 | 100<BinSelect < 125 |
| Bin2 | 125<BinSelect < 150 |
| Bin3 | 150<BinSelect < 175 |
| Bin4 | 175<BinSelect < 200 |

A bin's value is incremented whenever the BinSelect variable's value falls within the range associated with that bin.

A weighted histogram can be created by using a variable for the WtVal argument. The WtVal variable's value can be tied to a sensor's output or it can be set by the program according to existing conditions. When the BinSelect value falls within a bin's range, the total for that bin is incremented by the current value of the WtVal variable. If a weighted histogram is not desired, WtVal should be set to a constant value. Set WtVal to 1 to increment the bin by 1 for each occurrence.

The process string for the Histogram instructions is as follows:

Process Name (HST), Number of Bins, Weighting Value, Low Limit, High Limit

The Histogram is reset when the data is output to a table. It can also be reset under program control by setting the DisableVar to 12345.

## Parameters

# BinSelect (Bin Select)

The variable that is tested to determine which bin is selected. Right-click the parameter to display a list of defined variables.

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

If 12345 is entered for the DisableVar, the histogram is reset after output (regardless of the form used). If -12345 is entered, the histogram is reset immediately.

# Bins (or Subranges)

The number of bins or subranges to include in the histogram. The width of each subrange is equal to the histogram range (UpLim - LoLim) divided by the number of bins.

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

# LoLim (Lower Limit)

The lower limit of the range covered by the BinSelect value.

Type: Constant (or expression that evaluates as a constant)

# UpLim (Upper Limit)

The upper limit of the range covered by the BinSelect value.

Type: Constant (or expression that evaluates as a constant)
