# RainFlow (Rainflow)

The RainFlow instruction is used to process data with the rainflow counting algorithm. The output is a one or two dimensional rainflow histogram.

## Syntax

RainFlow(Source,DataType,DisableVar,MeanBins,AmpBins,LoLim,UpLim,MinAmp,Form)

Following is sample code using the RainFlow output instruction.

```
'\\\\\\\\\\\\\\\\\\\\\\\ TIMING CONSTANTS ///////////////////////
Const PERIOD = 50'Scan interval number
Const P_UNITS = 1'Scan interval units (mSec)
Const INTERVAL1 = 2'Table 1 interval number
Const UNITS1 = 2'Table 1 interval units (Sec)

'\\\\\\\\\\\\\\\\\\\\\\ VOLTAGE CONSTANTS //////////////////////
'________________________ Volt Block #1 ________________________
Const VRNG1 =mv2500'Block #1 measurement rangemv2500
Const VREP1 = 1'Block #1 repetitions
Const VSETL1 = 0'Block #1 settling time (usec)
Const VINT1 = 60'Block #1 integration time (usec)
Const VMULT1 = 1'Block #1 default multiplier
Const VOSET1 = 0'Block #1 default offset
Public VBlk1(VREP1)'Block #1 dimensioned source
Units VBlk1 = mVolts'Block #1 default units (mVolts)

'\\\\\\\\\\\\\\\\\\ ALIASES & OTHER VARIABLES //////////////////
Alias VBlk1(1) = Test'Assign alias name Test to VBlk1(1)

'\\\\\\\\\\\\\\\\\\\\\\\\ OUTPUT SECTION ////////////////////////
'---------------------------- Table #1----------------------------
DataTable(Table1,True,-1)'Trigger, auto size
DataInterval(0,INTERVAL1,UNITS1,10)'2 Sec Scan,10 lapses,autosize
'______________________ Voltage Blocks ______________________
RainFlow(VBlk1(1),IEEE4,False,10,10,500,4500,50,000)
EndTable'End of table Table1

'\\\\\\\\\\\\\\\\\\\\\\\\\\\\ PROGRAM ////////////////////////////
BeginProg'Program begins here
Scan(PERIOD,P_UNITS,0,0)'Scan once every 2mSec, non-burst
'__________________________ Volt Blocks __________________________
VoltDiff(VBlk1(),VREP1,VRNG1,1,False,VSETL1,VINT1,VMULT1,VOSET1)

CallTable Table1'Run Table Table1
NextScan'Loop up for the next scan
EndProg
'***** Program End *****
```

## Remarks

This output instruction is essential to estimating cumulative damage fatigue to components undergoing stress/strain cycles. The input signal (Source) is processed into a two dimensional rainflow histogram. One dimension represents the amplitude of the closed loop cycle (i.e., the distance between the cycle's peak and valley values). The other dimension represents the mean value of the cycle (i.e., [peak value + valley value]/2).

The value recorded in each element (bin) of the histogram can be either the actual number of closed loop cycles that had the amplitude and mean value associated with that bin, or the ratio of the number of cycles having mean and amplitude values in the specific bin's range with respect to the total number of cycles that were counted (i.e., number of cycles in bin divided by total number of cycles counted).

The range sizes for the amplitude columns are calculated by taking the difference between the upper (UpLim) and lower (LoLim) limits of the Mean values and dividing by the number of amp ranges (AmpBins).

The histogram can have either open or closed form. In the open form, a cycle that has an amplitude larger than the maximum bin is counted in the maximum bin; a cycle that has a mean value less than the lower limit or greater than the upper limit is counted in the minimum or maximum mean bin. In the closed form, a cycle that is beyond the amplitude or mean limits is not counted.

The algorithm for this instruction is based on the work done by Stephen Downing and Darrell Socie, which is documented in Volume 4 Issue 1 of the International Journal of Fatigue (Jan 1982).

## Parameters

# Source

The name of the Variable that is tested to determine which bin is selected. Right-click the parameter to display a list of defined variables.

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

# DisableVar

A Constant, Variable, or Expression that is used to determine whether the current measurement is included in the output saved to the DataTable. Right-click the parameter to display a list.

- 0 = Process current measurement

- Non-zero = Do not process current measurement.

** NOTE:**For all instructions except Totalize, if the disable variable is true over the entire output interval,NAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

will be stored. For Totalize, if the disable variable is true over the entire output interval, 0 will be stored

[See alsoMultipliers, Offsets, and Disable Variables with Repetitions.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Expression

# MeanBins (Mean Bins)

The number of rows or fields to parse the stress/strain cycle's mean value into. Enter 1 to disregard the mean value and sort only by the amplitude of the stress/strain cycle. The range of each row of bins is equal to the UpLimit minus the LoLimit divided by the MeanBin's value. The lowest mean row s minimum boundary is the LoLim and the highest mean row s maximum boundary is the UpLim.

Type: Constant (or expression that evaluates as a constant)

# AmpBins (Amplitude Bins)

The number of columns or fields to parse the amplitude of the stress/strain cycle into. The range of each Amp column is equal to the HiLim minus the LoLim divided by the AmpBin's value. The lowest Amp column s minimum boundary is zero and the highest Amp column s maximum boundary is the HiLim minus the LoLim.

Type: Constant (or expression that evaluates as a constant)

# LoLim (Lower Limit)

LoLim is the lower limit of the input signal and the MeanBins.

Type: Constant (or expression that evaluates as a constant)

# UpLim (Upper Limit)

UpLim is the upper limit of the input signal and the MeanBins.

Type: Constant (or expression that evaluates as a constant)

# MinAmp (Minimum Amplitude)

The minimum amplitude that a stress/strain cycle must have to be counted. The MinAmp's value should be less than the band size of the amplitude columns ( [HiLim - LoLim]/[AmpBins] ) or else cycles having the amplitude specified for the first column of bins will not be counted. If the MinAmp's value is set too small, processing time will be consumed counting cycles which are in reality just noise.

Type: Constant (or expression that evaluates as a constant)

# Form

The Form consists of three elements: ABC. Right-click the parameter to display a list.

| Code  | Form                                                 |
| ----- | ---------------------------------------------------- |
| A = 0 | Reset histogram after each output.                   |
| A = 1 | Do not reset histogram.                              |
| B = 0 | Divide bins by total count.                          |
| B = 1 | Output total in each bin.                            |
| C = 0 | Open form. Include outside range values in end bins. |
| C = 1 | Closed form. Exclude values outside range.           |

Type: Constant

Note: When a histogram is reset, a new histogram is created and stored in the data table at the data table's output interval. The number of new data points that are used for calculating subsequent histograms is determined by the ratio of the data table output rate to the scan interval: (data table output rate)/(scan interval). Choose whether or not the histogram should be reset (all bins are set to zero) after each output. If the histogram is not reset, the values in each bin will continue to accumulate as long as the program runs.

Output

If the number of mean ranges equals M, and the number of amplitude ranges equals A, then the output is arranged sequentially in the order [C(1,1), C(1,2), C(1,A), C(2,1), C(2,2), C(2,3), C(M,1), C(M,2) . C(M,A) ]. Shown in a two dimensional array, the output would look like:

| Amplitude Values |                   |        |
| ---------------- | ----------------- | ------ |
| C(1,1), C(1,2)   | **. . . . . . .** | C(1,A) |
| C(2,1), C(2,2)   | **. . . . . . .** | C(2,A) |
| Mean             | . . . . . . .     | -      |
| Values           | . . . . . . .     | -      |
| C(M,1), C(M,2)   | . . . . . . .     | C(M,A) |

Example

| Parameter Settings                   |             |
| ------------------------------------ | ----------- |
| Mean's lower limit is set at 100     | LowLim= 100 |
| Mean's upper limit is set at 200     | UpLim = 200 |
| The number of mean rows is 2         | MeanDim = 2 |
| The number of amplitude columns is 5 | AmpDim = 5  |

| Resultant Amp Settings                                        |           |       |
| ------------------------------------------------------------- | --------- | ----- |
| Full amplitude range is 100                                   | 200 - 100 | = 100 |
| Individual amplitude column size is 20                        | 100/5     | = 20  |
| First column of bins includes cycles having amplitude values: | 0 ≤ A     | < 20  |
| Second column of bins have cycle amplitude values:            | 20 ≤ A    | < 40  |
| Third column of bins have cycle amplitude values:             | 40 ≤ A    | < 60  |
| Fourth column of bins have cycle amplitude values:            | 60 ≤ A    | < 80  |
| Fifth column of bins have cycle amplitude values:             | 80 ≤ A    | <100  |

| Resultant Mean Row Settings                           |           |       |
| ----------------------------------------------------- | --------- | ----- |
| Full mean range is 100                                | 200 - 100 | = 100 |
| Individual mean bin row range is 50                   | 100/2     | = 50  |
| First row of bins includes cycles having mean values: | 100 ≤ M   | < 150 |
| Second row of bins have cycle mean values:            | 150 ≤ M   | < 200 |

In the previous example, the count would go to output bin:

|        |      |                |     |                  |
| ------ | ---- | -------------- | --- | ---------------- |
| C(1,1) | when | 0 ≤ Amp < 20   | and | 100 ≤ Mean < 150 |
| C(1,2) | when | 20 ≤ Amp < 40  | and | 100 ≤ Mean < 150 |
| C(1,3) | when | 40 ≤ Amp < 60  | and | 100 ≤ Mean < 150 |
| C(1,4) | when | 60 ≤ Amp < 80  | and | 100 ≤ Mean < 150 |
| C(1,5) | when | 80 ≤ Amp < 100 | and | 100 ≤ Mean < 150 |
| C(2,1) | when | 0 ≤ Amp < 20   | and | 150 ≤ Mean < 200 |
| C(2,2) | when | 20 ≤ Amp < 40  | and | 150 ≤ Mean < 200 |
| C(2,3) | when | 40 ≤ Amp < 60  | and | 150 ≤ Mean < 200 |
| C(2,4) | when | 60 ≤ Amp < 80  | and | 150 ≤ Mean < 200 |
| C(2,5) | when | 80 ≤ Amp < 100 | and | 150 ≤ Mean < 200 |
