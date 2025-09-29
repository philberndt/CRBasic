# LevelCrossing (Level Crossing)

The LevelCrossing instruction is used to process data into a one or two dimensional histogram using a LevelCrossing counting algorithm.

## Syntax

LevelCrossing(Source,DataType,DisableVar,NumLevels,SecondDim,CrossingArray,SecondArray,Hysteresis,Option)

Following is sample code using the LevelCrossing Instruction.

```
Const PERIOD = 200'Scan interval number
Const P_UNITS = 1'Scan interval units (mSec)
Const INTERVAL1 = 2'Table 1 interval number
Const UNITS1 = 2'Table 1 interval units (Sec)
'\\\\\\\\\\\\\\\\\\\\ THERMOCOUPLE CONSTANTS ////////////////////
'________________________ Temp Block #1 ________________________
Const TRNG1 =mv34'Block #1 measurement range (mv34)
Const TTYPE1 = TypeT'Block #1 thermocouple type (T)
Const TREP1 = 1'Block #1 repetitions
Const TSETL1 = 500'Block #1 settling time (usec)
Const TINT1 =4000'Block #1 integration
Const TMULT1 = 1.8'Block #1 default multiplier
Const TOSET1 = 32'Block #1 default offset
Dim TBlk1(TREP1)'Block #1 dimensioned source
Units TBlk1 = Deg_F'Block #1 default units (Deg_F)
'\\\\\\\\\\\\\\\\\\\\\\\ BRIDGE CONSTANTS ///////////////////////
'________________________ Bridge Block #1 ________________________
Const BRNG1 =mv34'Block #1 measurement range (mv2500)
Const BREP1 = 1'Block #1 repetitions
Const BEXCIT1 = 2500'Block #1 excitation mVolts
Const BSETL1 = 200'Block #1 settling time (usec)
Const BINT1 =4000'Block #1 integration time (usec)
Const BMULT1 = 1'Block #1 default multiplier
Const BOSET1 = 0'Block #1 default offset
Dim BBlk1(BREP1)'Block #1 dimensioned source
Units BBlk1 = Gforce'Block #1 default units (GForce)
Dim BBlk1LC1(11)'Block #1 level crossing array 1
Data -5,-4,-3,-2,-1,0,1,2,3,4,5'Block #1 data for array 1
Dim BBlk1LC2(6)'Block #1 level crossing array 2
Data 70,75,80,85,90,95'Block #1 data for array 2
Dim LCBBlk1_1(2)'Block #1 source for level crossing
'\\\\\\\\\\\\\\\\\\ ALIASES & OTHER VARIABLES //////////////////
Alias TBlk1(1) = coolant'Assign alias name coolant to TBlk1(1)
Alias BBlk1(1) = Accel1'Assign alias name Accel1 to BBlk1(1)
Public Flag(8)'General Purpose Flags
Dim I'Declare I as a variable
Dim TRef(1)'Declare Reference Temp variable
'\\\\\\\\\\\\\\\\\\\\\\\\ OUTPUT SECTION ////////////////////////
'---------------------------- Table #1----------------------------
DataTable(MAIN,True,50) 'Trigger, 50 records
DataInterval(0,INTERVAL1,UNITS1,5)'2 Sec interval, 5 lapses, 1.67 Mins
'_______________________ Bridge Blocks _______________________
LevelCrossing(LCBBlk1_1(),IEEE4,False,11,6,BBlk1LC1,BBlk1LC2,0.1,111)
EndTable
'\\\\\\\\\\\\\\\\\\\\\\\\\\\\ PROGRAM ////////////////////////////
BeginProg'Program begins here
For I = 1 To 11'Do to all of BBlk1LC1
Read BBlk1LC1(I)'Read data into BBlk1LC1
Next I'Repeat above until finished
For I = 1 To 6'Do to all of BBlk1LC2
Read BBlk1LC2(I)'Read data into BBlk1LC2
Next I'Repeat above until finished
Scan(PERIOD,P_UNITS,0,0)'Scan once every 2mSec, non-burst
PanelTemp(TRef(),20)'RefTemp,Integrate

TCDiff(TBlk1(),TREP1,TRNG1,1,TTYPE1,TRef(1),True,TSETL1,TINT1,TMULT1,TOSET1)

BrFull(BBlk1(),BREP1,BRNG1,1,Vx1,1,BEXCIT1,False,False,BSETL1,BINT1,BMULT1,BOSET1)

LCBBlk1_1(1) = BBlk1(1)'1st source dimension for LCBBlk1_1()
LCBBlk1_1(2) = Coolant'2nd source dimension for LCBBlk1_1()
CallTable MAIN'Run Table MAIN
NextScan'Loop up for the next scan
EndProg'Program ends here
'***** Program End *****
```

## Remarks

The first dimension of the histogram created by this instruction is the levels crossed. The second dimension, if used, is the value of a second input at the time the crossings are detected.

The crossing levels (CrossingArray) for the first source element and the upper boundary levels (SecondArray) for the second source element are contained in variable arrays. This permits the crossing and boundary level values to be loaded into the arrays by the program. If a second array is used (SecondDim > 1, with values loaded into SecondArray), a two dimensional histogram is created. The second source element is compared to the boundary values in the SecondArray only when a Level Crossing by the first source element has occurred. The levels should be loaded into the arrays sequentially from the lowest values to the highest.

## Parameters

# Source

The name of the first Variable that is the input for the instruction. If a two dimensional Level Crossing Histogram is desired, then the source must be at least a two dimensional array. The array element specified in the Source argument will be compared to the crossing levels (set in CrossingArray). The next element of the source array will be compared to the boundary levels set by the SecondArray.

The source value(s) may be the result of a measurement or calculation. When a DataTable having a Level Crossing instruction is called, the Source's first element is checked to see if its value has changed from the previous value. Only when the value of the first Source element crosses one or more of the levels set by the Crossing Array, is the count of one or more (depending upon how many levels are crossed) of the histogram bins incremented. The second Source element is compared to the values in the SecondArray only when a level crossing by the first source element has occurred.

Right-click the parameter to display a list of defined variables.

Type: Variable or Array

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

# NumLevels (Number of Crossing Levels)

The NumLevels argument determines the number of levels on which to count crossings. This is the number of bins in which to store the number of crossings for the associated level. The number of crossing levels that the first dimension has is set by the NumLevels argument. The Crossing Array must have as many array elements as the numeric value of the NumLevels argument.

Type: Constant (or expression that evaluates as a constant)

# SecondDim (Second Dimension)

The number of boundary level values too which the second Source element will be compared. The second dimension is separated into a set by the SecondDim argument. The SecondArray must have as many array elements as the numeric value of the SecondDim argument. If a one dimensional Level Crossing histogram is desired, this argument should be set to a value of 1, and the second array should be left empty. If a two dimensional LevelCrossing histogram is desired, then the SecondDim argument must be greater than one. The actual Boundary levels should be loaded into the SecondArray argument.

Type: Constant (or expression that evaluates as a constant)

# CrossingArray (Crossing Levels Array)

The name of the array that contains the Crossing levels to compare the first element of the Source array against. The first dimension of the histogram is broken up into discrete Crossing Levels according to the values in the Crossing Array. The number of Crossing Levels is set by the NumLevels argument. Therefore, the Crossing Array must be dimensioned to at least the value of the NumLevels argument. The levels should be loaded into the arrays sequentially from the lowest values to the highest. Right-click the parameter to display a list of defined variables.

Type: Array

# SecondArray (Second Array)

The name of the array that contains the upper bin limits to compare the second element of the Source array against. The first range is everything below the first element in the SecondArray. The second range is all values greater than or equal to the first element and less than the second element of the SecondArray; and so on. The number of ranges that the second dimension has is determined by the SecondDim argument, so this array must be dimensioned to at least the value of the SecondDim argument. The boundary levels should be loaded into the arrays sequentially from the lowest values to the highest. Because it does not make sense to change the levels while the program is running, the program should be written to load the values into the array once before entering the scan. Right-click the parameter to display a list of defined variables.

Type: Array

# Hysteresis

The Hysteresis determines the minimum change in the input that must occur before a crossing is counted. If this value is too small, noise could be counted as crossings. For example, suppose 5 is a crossing level. If the input is not really changing but is varying from 4.999 to 5.001, a Hysteresis of 0 would allow all these crossings to be counted. Setting the Hysteresis to 0.1 would prevent this noise from causing counts.

Type: Constant (or expression that evaluates as a constant)

# Option

The Option argument can program the level crossing histogram to output the actual number of times that the signal crossed the level associated with an individual bin, or it can be set to output the fraction of crossings associated to that bin with respect to the total number of crossings (number of crossings associated with an individual bin divided by the total number of crossings associated with all of the bins). The option argument is also used to program the histogram to count on either the rising or falling edge, and whether or not to reset the histogram after each output. The Option Codeconsists of three elements: ABC. Right-click the parameter to display a list.

| Code  | Form                                                            |
| ----- | --------------------------------------------------------------- |
| A = 0 | Count on falling edge.                                          |
| A = 1 | Count on rising edge.                                           |
| B = 0 | Reset histogram counts to 0 after each output.                  |
| B = 1 | Do not reset histogram counts.                                  |
| C = 0 | Divide count in each bin by total number of counts in all bins. |
| C = 1 | Output total counts in each bin.                                |

Type: Constant

** NOTE:**When a histogram is reset, a new histogram is created and stored in the data table at the data table's output interval. The number of new data points that are used for calculating subsequent histograms is determined by the ratio of the data table output rate to the scan interval: (data table output rate)/(scan interval). Choose whether or not the histogram should be reset (all bins are set to zero) after each output. If the histogram is not reset, the values in each bin will continue to accumulate as long as the program runs.

## Output

If the number of Level Crossing values equals L (NumLevels = L), and the number of secondary ranges equals R (SecondDim = R), then the total number of bins would be the product of L and R. The output is arranged sequentially in the order [Bin(1,1), Bin(1,2), Bin(1,R), Bin(2,1), Bin(2,2), Bin(2,3), Bin(L,1), Bin(L,2) . Bin(L,R) ]. Shown in a two dimensional array, the output would look like:

|                           |               |
| ------------------------- | ------------- | -------- |
| Second Dimensional Values |               |
| Bin(1,1), Bin(1,2)        | . . . . . . . | Bin(1,R) |
| Bin(2,1), Bin(2,2)        | . . . . . . . | Bin(2,R) |
| Level                     | . . . . . . . | -        |
| Crossing                  | . . . . . . . | -        |
| Values                    |
| Bin(L,1), Bin(L,2)        | . . . . . . . | Bin(L,R) |
