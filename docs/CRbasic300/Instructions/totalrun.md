# TotalRun (Running Total)

The TotalRun instruction is used to output a running total of a measurement.

## Syntax

TotalRun(Dest,Reps,Source,Number,RunReset[optional],Count[optional], TotalCalls[optional],Call_ID[optional])

The following simple program shows the use of the TotalRun instruction. A running total is kept over 100 values.The RunTotal variable is increment by 2 with each program scan. The running total can be reset by setting the Boolean value, RunReset, to True, or it is set automatically under program control once the running total equals 200.

```
Public RunReset As Boolean, Counter As Long, RunTotal 'As Long

DataTable (Count,True,-1)
DataInterval (0,5,sec,10)
Sample (1,Counter,Long)
EndTable

BeginProg
Scan (1,Sec,3,0)
Counter=2
TotalRun(RunTotal,1,Counter,100,RunReset OR RunTotal>=200)
If RunReset=True Then RunReset=False
CallTable (Count)
NextScan
EndProg
```

## Remarks

This instruction, when used, must appear between the Scan NextScan instructions. A running total is the total of the last N values where N is specified in the Number argument. Until the datalogger has taken N measurements the total is calculated based on the number of actual measurements made.

NAN values are not included in the running total. However, they can affect the running total by reducing the number of values contained in the total. The running total is calculated from the non-NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

values in the buffer of historical data. Note that all values, including NAN, are stored in the ring buffer. When a new value comes in, the running total is calculated by adding in the new value (if not NAN) and subtracting the oldest value (if not NAN). For example, if you have an N (number of values) of 5, but 2 of those are NANs, then N is reduced to 3, which results in a poorer total. In the event that all values in the buffer are NAN, a NAN is returned as the running total (this can be useful in detecting faults; for example, a broken sensor). If you want to ensure there are always N values used in the running total, theTotalRun instruction should be called conditionally (only when good values are received). This will eliminate any memory in the buffer being allocated to NAN values that are not used in the calculation. Calling the TotalRun instruction conditionally could also be used to remove values that are out-of-range.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the TotalRun instruction, the Reps parameter is the number of variables in the array for which a running total should be calculated. When the Source is not an array, or only a single variable in the array should be totaled, Reps should be 1.

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

# Number

The number of values of [Source](../parameters/source.md) to be used for calculating the running average, running total, running minimum, running maximum, or running standard deviation.

Type: Constant (or expression that evaluates as a constant)

## Optional Parameter

# RunReset

When RunReset is true, the history over which the running value (for example, running average, running maximum, running minimum, running total, or running standard deviation) is calculated is cleared, so that the value returned is calculated from a a single value, the current input. Note that the reset parameter does not automatically toggle back to false once set to true. When the reset parameter is set back to false (problematically or manually), then the running average, running maximum, running minimum, or running total continues, starting with the current input. Note that until the data logger has taken N measurements, the running average/total/maximum/minimum is calculated based on the actual number of measurements made since reset was set back to false.

Type:Boolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

variable or expression that evaluates true/false

# Count

Optional parameteradded in OS9.01and greaterthat returns the actual number of values used to calculate the running value. The actual number of values used may be different than the number specified for the calculation due to NAN values in the buffer of historical data. The Count parameter can only be used if the optional Reset parameter is also used. The Count parameter is a variable of Type Long and must be the same dimension as the array that is being averaged (i.e., the same as reps). A count used for each rep will be written to this variable on each execution of the instruction.

The TotalCalls and Call_ID optional parameters below, allow a single instance of the instruction ([AvgRun](avgrun.md),[MaxRun](maxrun.md),[MinRun](minrun.md),[StdDevRun](stddevrun.md)) to be called with different source variables, as in a subroutine or function. Click [here](../parameters/TotalCalls_CallID_Explanation.md) for additional information. These parameters are available with operating system11and later.

# TotalCalls

Optional parameter that specifies the total number of calls to a specific instance of the instruction ([AvgRun](avgrun.md),[MaxRun](maxrun.md),[MinRun](minrun.md),[StdDevRun](stddevrun.md)) in the program.

Type: Constant integer

# Call_ID

Specifies a unique ID number for each specific call of the instruction.

Type: Integer

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [Average](average.md),[AvgRun](avgrun.md),[AvgSpa](avgspa.md),[CovSpa](covspa.md),[MinRun](minrun.md),[MaxRun](maxrun.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevRun](stddevrun.md),[StdDevSpa](stddevspa.md),[TotalRun](#), and [Totalize](totalize.md).
