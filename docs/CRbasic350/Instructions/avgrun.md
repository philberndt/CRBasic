# AvgRun (Running Average)

The AvgRun instruction is used to output a running average of a measurement.

## Syntax

AvgRun(Dest,Reps,Source,Number,RunReset[optional],Count[optional], TotalCalls[optional],Call_ID[optional] )

The following code shows the use of the AvgRun instruction. The running average for a TC measurement is stored in a variable called RunAvgTC and then sampled to a data table. In this example, AvgRun is called conditionally to avoid NAN values affecting the average.

```
'CR350 SeriesDatalogger

Public PTemp, TCTemp, RunAvgTC

DataTable (TCTemp,True,-1)
DataInterval (0,1,Min,10)
Sample (1,RunAvgTC(),FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

If TCTemp <> NAN Then'call AvgRun conditionally
AvgRun(RunAvgTC,1,TCTemp,60)
EndIf
CallTable (TCTemp)
NextScan
EndProg
```

### **Running Total Example** In the example below, AvgRun is used to calculate a 24 hour running total. Note that the variable "count" is used for the multiplier to derive the running total, rather than a constant. The program will need to run a full 24 hours (1440 minutes) before a true running total is calculated.

```
Public MeasuredRain
Public RainSum, RainAverage, Count

BeginProg
Count=0
Scan(1,Min,0,0)
PulseCount (MeasuredRain,1,C1,2,0,0.01,0)
AvgRun(RainAverage,1,MeasuredRain,1440)
If Count<1440 Then Count=Count+1
RainSum=Count*RainAverage
NextScan
EndProg
```

### **Optional Reset and count Parameters Examples** This example demonstrates how the optional Reset and Count parameters work. The program runs for 22 scans. On the 11th scan, the reset parameter is used to clear the running average buffer and start the calculation over, beginning with the11th value. The optional Count parameter returns the actual number of values used in the calculation. In this example, at the end of 22 scans, AvgCnt1= 12, because 12 values were used in the calculation. AvgCnt2 = 11 because one of the values after the reset was NaN, which was not used in the calculation.

```
Public Avg1, Avg2
Public Reset as Boolean'The Reset parameter must be of Type Boolean
Public AvgCnt1 As Long,AvgCnt2 As Long'The Count parameter must be of Type Long
Dim i
'Provide some values for the AvgRun calculation
Dim NANBeg(22) = {NAN,6.2,7.3,8.4,9.5,10.6,11.7,12.8,13.9,15,16.1,17.2,18.3,19.4,20.5,21.6,22.7,23.8,24.9,26,27.1,28.2}
Dim NANEnd(22) = {5.1,6.2,7.3,8.4,9.5,10.6,11.7,12.8,13.9,15,16.1,17.2,18.3,19.4,20.5,21.6,22.7,23.8,24.9,26,27.1, NAN}

BeginProg
Scan (1,Sec,3,22)
'Fill the AvgRun variable arrays with the values provided above and set the
'Reset variable to 11.
i+=1
If i=11 Then Reset = True Else Reset = False
'After 22 scans, AvgCnt1 will = 12 because after the Reset, 12 values are used to calculate the running average.
AvgRun(Avg1,1,NANBeg(i),100,Reset,AvgCnt1)
'After 22 scans, AcgCnt2 will = 11 because after the Reset, 11 values are used to calculate the running average due to a NAN within the reset range.
AvgRun (Avg2,1,NANEnd(i),100,Reset,AvgCnt2)
NextScan
EndProg
```

### **OPTIONAL TOTALCALLS AND CALL_ID EXAMPLE** The following program demonstrates using the optional parameters TotalCalls and Call_ID to calculate running averages for 3 source variables, X, Y, and Z in a subroutine.

```
Public X As Float
Const X_ID = 1
Public Y As Float
Const Y_ID = 2
Public Z As Float
Const Z_ID = 3
Public X_avg_sub
Public Y_avg_sub
Public Z_avg_sub
Public Reset_Runs As Boolean = false
Sub run(ResultAvg As Float, source As Float, sourceID)
AvgRun(ResultAvg,1,source,10,Reset_Runs,3, sourceID)'TotalCalls = 3
EndSub

'Main Program
BeginProg
Scan (1,Sec,0,0)
X += 1'generate some data for the source variables for demonstration
Y += 2
Z += .25
'Call subroutine 3 times (once for each source variable)
Call run(X_avg_sub,X,X_ID)
Call run(Y_avg_sub,Y,Y_ID)
Call run(Z_avg_sub,Z,Z_ID)
NextScan
EndProg
```

## Remarks

This instruction, when used, must appear between the Scan NextScan instructions. A running average is the average of the last N values where N is specified in the Number argument.

_Xn _ is the most recent value of the Source and*Xn-1 * is the previous value (_X1 _ is the oldest value included in the average, i.e., N-1 values from the most recent). Until the datalogger has taken N measurements the average is calculated based on the number of actual measurements made.

NAN values are not included in the running average. However, they can affect the running average by reducing the number of values contained in the average. The running average is calculated from the non-NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

values in the buffer of historical data. Note that all values, including NAN, are stored in the ring buffer. When a new value comes in, the running average is calculated by adding in the new value (if not NAN) and subtracting the oldest value (if not NAN) and then dividing by the number of values in the buffer that are not NAN. For example, if you have an N (number of values) of 5, but 2 of those are NANs, then N is reduced to 3, which results in a poorer average. In the event that all values in the buffer are NAN, a NAN is returned as the running average (this can be useful in detecting faults; for example, a broken sensor). If you want to ensure there are always N values used in the running average, the AvgRun instruction should be called conditionally (only when good values are received). This will eliminate any memory in the buffer being allocated to NAN values that are not used in the calculation. Calling the AvgRun instruction conditionally could also be used to remove values that are out-of-range.

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

.

**NOTE:** This instruction normally should not be inserted within a For/Next construct with the Source and Destination parameters indexed and Reps set to 1. In essence this would perform a single running average, using the values of the different elements of the array, instead of performing an independent running average on each element of the array. The results of this would be a Running Average of a Spatial Average on the various Source array's elements.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

The Reps parameter is the number of variables in the array for which a running average should be calculated. When the Source is not an array, only a single variable in the array should be averaged, Reps should be 1.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

For the AvgRun instruction, the Source parameter is the name of the variable for which a running average should be calculated.

# Number

The number of values of [Source](../parameters/source.md) to be used for calculating the running average, running total, running minimum, running maximum, or running standard deviation.

Type: Constant (or expression that evaluates as a constant)

## Optional Parameters

# RunReset

When RunReset is true, the history over which the running value (for example, running average, running maximum, running minimum, running total, or running standard deviation) is calculated is cleared, so that the value returned is calculated from a a single value, the current input. Note that the reset parameter does not automatically toggle back to false once set to true. When the reset parameter is set back to false (problematically or manually), then the running average, running maximum, running minimum, or running total continues, starting with the current input. Note that until the data logger has taken N measurements, the running average/total/maximum/minimum is calculated based on the actual number of measurements made since reset was set back to false.

Type:Boolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

variable or expression that evaluates true/false

# Count

Optional parameter that returns the actual number of values used to calculate the running value. The actual number of values used may be different than the number specified for the calculation due to NAN values in the buffer of historical data. The Count parameter can only be used if the optional Reset parameter is also used. The Count parameter is a variable of Type Long and must be the same dimension as the array that is being averaged (i.e., the same as reps). A count used for each rep will be written to this variable on each execution of the instruction.

The TotalCalls and Call_ID optional parameters below, allow a single instance of the instruction ([AvgRun](#),[MaxRun](maxrun.md),[MinRun](minrun.md),[StdDevRun](stddevrun.md)) to be called with different source variables, as in a subroutine or function. Click [here](../parameters/TotalCalls_CallID_Explanation.md) for additional information. These parameters are available with operating system2and later.

# TotalCalls

Optional parameter that specifies the total number of calls to a specific instance of the instruction ([AvgRun](#),[MaxRun](maxrun.md),[MinRun](minrun.md),[StdDevRun](stddevrun.md)) in the program.

Type: Constant integer

# Call_ID

Specifies a unique ID number for each specific call of the instruction.

Type: Integer

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [Average](average.md),[AvgRun](#),[AvgSpa](avgspa.md),[CovSpa](covspa.md),[MinRun](minrun.md),[MaxRun](maxrun.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevRun](stddevrun.md),[StdDevSpa](stddevspa.md),[TotalRun](totalrun.md), and [Totalize](totalize.md).
