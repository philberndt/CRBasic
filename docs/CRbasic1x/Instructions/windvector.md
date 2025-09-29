# WindVector (Calculate Wind Vector)

The WindVector instruction processes the raw data from wind speed and wind direction measurements to generate the mean wind speed, mean wind vector magnitude, mean wind vector direction, and standard deviation of wind vector direction.

## Syntax

```
WindVector
```

(Reps,Speed/East,Direction/North,DataType,DisableVar,Subinterval,SensorType,OutputOpt)

The following program measures a wind speed and direction sensor. Wind vector is stored hourly. The Fieldnames instruction is used to assign unique names to the three wind vector outputs.

```
'CR1000X
' Example program to measure the 05103 RM Young Wind Monitor using the
' BrHalf measurement instruction and the WindVector instruction to
' process and store wind speed, direction, and standard deviation.

'Declare Variables and Units
Public WS_ms
Public WindDir
Units WS_ms=meters/second
Units WindDir=Degrees

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
WindVector(1,WS_ms,WindDir,IEEE4,0,0,0,0)
FieldNames("WS_ms_S_WVT,WindDir_D1_WVT,WindDir_SD1_WVT")
EndTable

'Main Program
BeginProg
Scan(5,Sec,1,0)
'05103 Wind Speed & Direction Sensor measurements WS_ms and WindDir:
PulseCount(WS_ms,1,P1,1,1,0.098,0)
BrHalf(WindDir,1,mv5000C,1,Vx1,1,2500,True,0,15000,355,0)

'Call Data Tables and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

Data can be processed from either polar (wind speed and direction) or orthogonal (fixed East and North propellers) wind sensors. Two different algorithms are available to calculate wind vector direction, one of which is weighted for wind speed.

### NAN Values

Call the output table conditionally (for example, do not call the table if a variable =NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

) to keep NANs from affecting the other good values.

If the DisableVar is used to keep NAN values from affecting output, but no valid values are measured for the period, then NAN is stored.

If the DisableVar is true for the entire output interval, NAN is stored.

If one or more NAN values are included in the wind speed values being processed, NAN is output for wind speed, 0 is stored for wind direction and standard deviation, and NAN is stored for mean wind vector magnitude.

If one or more NAN values are included in the wind direction values being processed, but the wind speed values are good, the good values will be output for wind speed, 0 is stored for wind direction and standard deviation, and NAN is stored for mean wind vector magnitude.

Note that since there is no such thing as NAN for integers, values that are converted from float to integer are expressed in data tables as the most negative number for a given data type. For example, the most negative number of data type FP2 is -7999, so NAN for FP2 data will appear in a data table as -7999. If the data type is Long, NAN will appear in the data table as -2147483648.

### Zero-Wind-Speed Measurements

When processing scalar mean wind speed, the WindVector() instruction includes any zero-wind-speed measurements. In contrast, because vectors require magnitude and direction, measurements at zero wind speed are not used in vector speed or direction calculations.

## Parameters

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the WindVector instruction, the Reps are the number of wind vector averages to calculate. If the Reps parameter is greater than 1, arrays must be specified for the Speed/East and Direction/North parameters. If not, a Variable Out of Bounds error is returned when the program is compiled.

# Speed/East (Wind Speed or East Direction)

Enter the source variable for the wind speed (for a polar sensor) or East direction (for an orthogonal sensor). If Reps is greater than 1, this parameter must be an array dimensioned large enough to accommodate the repetitions. This parameter is ignored at compile time when OutputOpt 3 or 4 is used. However, a wind speed variable that is dimensioned to at least the number of repetitions in the instruction must be entered or a compile error will result.

# Direction/North (Wind Direction or North Direction)

Enter the source variable for the wind direction (for a polar sensor) or North direction (for an orthogonal sensor). If Reps is greater than 1, this parameter must be an array dimensioned large enough to accommodate the repetitions.

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

# Subinterval

The Subinterval parameter is used to determine how the standard deviation will be averaged. 0 can be entered to use every sample taken during the output period, or a subinterval value can be entered to process standard deviations from shorter intervals of the output interval. Averaging subinterval standard deviations minimizes the effects of meander under light wind conditions.

Standard deviation of horizontal wind fluctuations from sub-intervals is calculated as follows:

Whereis the standard deviation over the data storage interval andare sub-interval standard deviations.

A subinterval is specified as a number of scans. The number of scans for a subinterval is given by:

Desired subinterval (sec) / scan rate (sec)

If the scan rate is 1 sec and the DataInterval is 60 minutes, the standard deviation is calculated for all 3600 scans when the subinterval is 0. With a subinterval of 900 scans, the standard deviation is the average of the four subinterval standard deviations. The last subinterval is weighted if it does not contain the specified number of scans.

# SensorType (Type of Sensor)

Used to define the type of sensor that is being measured. 0 = polar sensor; 1 = orthogonal sensor.

# OutputOpt (Output Options)

Used to define the values which will be stored. All output options for this instruction result in an output array. The elements of the array have \_WVc(n) as a suffix, where n is the element number. The array uses the name of the Speed/East variable as its base. For details on wind speed and unit vector calculations, see [Wind Vector Calculations](../Info/wv_calculations.md).

| Value | Description                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Mean horizontal wind speed (WVc(1)), unit vector mean wind direction (WVc(2)), and standard deviation of wind direction (WVc(3)). Standard deviation is calculated using the Yamartino algorithm. This option complies with EPA guidelines for use with straight-line Gaussian dispersion models to model plume transport.                                                                                                                           |
| 1     | Mean horizontal wind speed (WVc(1)) and unit vector mean wind direction (WVc(2)).                                                                                                                                                                                                                                                                                                                                                                    |
| 2     | Mean horizontal wind speed (WVc(1)), mean wind vector magnitude (WVc(2)), resultant mean wind direction (WVc(3)), and standard deviation of wind direction (WVc(4)). Standard deviation is calculated using theCampbell Scientificwind speed weighted algorithm. Use of the mean wind vector magnitude is not recommended for straight-line Gaussian dispersion models, but may be used to model transport direction in a variable-trajectory model. |
| 3     | Unit vector mean wind direction (WVc(1)).                                                                                                                                                                                                                                                                                                                                                                                                            |
| 4     | Unit vector mean wind direction (WVc(1)) and standard deviation of wind direction (WVc(2)). Standard deviation is calculated using Campbell Scientific's wind speed weighted algorithm. Use of the mean wind vector magnitude is not recommended for straight-line Gaussian dispersion models, but may be used to model transport direction in a variable-trajectory model.                                                                          |

The [FieldNames](fieldnames.md) instruction can be used to assign individual labels to the elements in the array. In addition, when FieldNames is used, the units for the Direction outputs will be listed as Degs in the data table header. The first output, speed, will reflect the [Units](units.md) designation for that variable if one is present in the program. If Units is not designated, the units field for that variable in the data table header will be blank. Note that a separate variable with a Units assignment can be dimensioned for wind speed, and that variable can be used in the FieldNames instruction; for example,

FieldNames ("mean_wind_speed, mean_wind_direction, std_wind_dir")

Dim mean_wind_speed : Units mean_wind_speed = m/s
