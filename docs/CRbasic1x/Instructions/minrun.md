# MinRun (Running Minimum)

The MinRun instruction is used to output the running minimum of a measurement.

## Syntax

MinRun(Dest,Reps,Source,Number,RunReset[optional], TotalCalls[optional],Call_ID[optional] )

The following simple program shows the use of the AvgRun,TotalRun, MinRun, and MaxRun instructions. A running value (running average, running total, running minimum, and running maximum) of a counter is kept over 9 values. If the Reset variable is set to true (for example, toggled 'true' via Numeric Monitor), then AvgRun, TotalRun, MinRun and MaxRun all return the same value (the current value), because the history over which the running value is calculated is cleared. Note that once Reset = True, it must be manually or programatically toggled back off (False). When Reset is toggled off (False), then calculation of the running value resumes, beginning with the current input. Note that until the datalogger has taken N measurements (in this case N = 9) the running value is based on the actual number of measurements made.

```
Public Counter As Long, Reset As Boolean, Run_Avg,Run_Min,Run_Max,Run_Total

'Main Program
BeginProg
Reset = false
Scan (1,Sec,0,0)
counter = counter + 1
AvgRun(Run_Avg,1,counter,9,Reset)'Average of last 9 values
TotalRun(Run_Total,1,counter,9,Reset)'Total of last 9 values
MinRun(Run_Min,1,counter,9,Reset)'Minimum of last 9 values
MaxRun(Run_Max,1,counter,9,Reset)'Maximum of last 9 values
NextScan
EndProg
```

### **OPTIONAL TOTALCALLS AND CALL_ID EXAMPLE** ```

The following program demonstrates using the optional parameters TotalCalls and Call_ID to calculate running minimum averages for 3 source variables, X, Y, and Z in a subroutine.

```
Public X As Float
Const X_ID = 1
Public Y As Float
Const Y_ID = 2
Public Z As Float
Const Z_ID = 3
Public X_min_sub
Public Y_min_sub
Public Z_min_sub
Public Reset_Runs As Boolean = false

Sub run(ResultMin As Float, source As Float, sourceID)
MinRun(ResultMin,1,source,10,Reset_Runs,3, sourceID)'TotalCalls = 3
EndSub

'Main Program
BeginProg
Scan (1,Sec,0,0)
X += 1'generate some data for the source variables for demonstration
Y += 2
Z += .25
'Call subroutine 3 times (once for each source variable)
Call run(X_min_sub,X,X_ID)
Call run(Y_min_sub,Y,Y_ID)
Call run(Z_min_sub,Z,Z_ID
NextScan
EndProg
```

## Remarks

This instruction, when used, must appear between the Scan NextScan instructions. A running minimum is the minimum of the last N values where N is specified in the Number argument. Until the data logger has taken N measurements the minimum is calculated based on the number of actual measurements made.

NAN values are not included in the running minimum. However, they can affect the running minimum by reducing the number of values contained in the minimum. The running minimum is calculated from the non-NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

values in the buffer of historical data. Note that all values, including NAN, are stored in the ring buffer. If you have an N (number of values) of 5, but 2 of those are NANs, then N is reduced to 3, which results in a poorer minimum. In the event that all values in the buffer are NAN, a NAN is returned as the running minimum (this can be useful in detecting faults; for example, a broken sensor). If you want to ensure there are always N values used in the running minimum, the MinRun instruction should be called conditionally (only when good values are received). This will eliminate any memory in the buffer being allocated to NAN values. Calling the MinRun instruction conditionally could also be used to remove values that are out-of-range.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the MinRun instruction, the Reps parameter is the number of variables in the array for which a running minimum should be calculated. When the Source is not an array, or only a single variable in the array should be used, Reps should be 1.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

# Number

The number of values of [Source](../parameters/source.md) to be used for calculating the running average, running total, running minimum, running maximum, or running standard deviation.

Type: Constant (or expression that evaluates as a constant)

## Optional Parameters

# RunReset

When RunReset is true, the history over which the running value (for example, running average, running maximum, running minimum, running total, or running standard deviation) is calculated is cleared, so that the value returned is calculated from a a single value, the current input. Note that the reset parameter does not automatically toggle back to false once set to true. When the reset parameter is set back to false (problematically or manually), then the running average, running maximum, running minimum, or running total continues, starting with the current input. Note that until the data logger has taken N measurements, the running average/total/maximum/minimum is calculated based on the actual number of measurements made since reset was set back to false.

Type:Boolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

variable or expression that evaluates true/false

The TotalCalls and Call_ID optional parameters below, allow a single instance of the instruction ([AvgRun](avgrun.md),[MaxRun](maxrun.md),[MinRun](#),[StdDevRun](stddevrun.md)) to be called with different source variables, as in a subroutine or function. Click [here](../parameters/TotalCalls_CallID_Explanation.md) for additional information. These parameters are available with operating system7and later.

# TotalCalls

Optional parameter that specifies the total number of calls to a specific instance of the instruction ([AvgRun](avgrun.md),[MaxRun](maxrun.md),[MinRun](#),[StdDevRun](stddevrun.md)) in the program.

Type: Constant integer

# Call_ID

Specifies a unique ID number for each specific call of the instruction.

Type: Integer

**NOTE:** If more than one instance of the instruction occurs in the program, each instance requires its own set of TotalCalls and Call_ID parameters.

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [AddPrecise](addprecise.md),[Average](average.md),[AvgRun](avgrun.md),[AvgSpa](avgspa.md),[CovSpa](covspa.md),[MinRun](#),[MaxRun](maxrun.md),[MovePrecise](moveprecise.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevRun](stddevrun.md),[StdDevSpa](stddevspa.md),[TotalRun](totalrun.md), and [Totalize](totalize.md).
