# ETsz (Evapotranspiration)

The ETsz instruction outputs water lost, in millimeters, due to evapotranspiration and calculated clear sky solar radiation.

## Syntax

ETsz(Temp,RH,uZ,Rs,Longitude,Latitude,Altitude,Zw,Sz,DataType,DisableVar)

The following program uses the ETsz instruction to output evapotranspiration and calculated solar radiation. Sample temperature, RH, solar radiation, and wind speed are also saved to the data table. The program measures temperature, RH, wind speed, and solar radiation; altitude, latitude, and longitude are input as constants. Wind speed sensor height is set to 3 meters prior to the first scan of the program; the variable can be changed if desired.

The sensor used for this example is a CS300. If a different sensor is used, a different multiplier would be used for calculating megajoules per meters squared and kilowatts per meters squared.

```
Public Temp, RH, WindSpeed, SlrKw,SlrMj, Zw
Const Altitude=1545
Const Latitude=41
Const Longitude=111

DataTable (MetData,1,2500)
DataInterval (0,1,Hr,10)
Sample (1,Temp,FP2)'Store sample temperature
Sample (1,RH,FP2)'Store sample RH
Sample (1,SlrMj,FP2)'Store sample solar radiation
Sample (1,WindSpeed,FP2)'store sample windspeed
ETsz(Temp,RH,WindSpeed,SlrMj,Longitude,Latitude,Altitude,Zw,0,FP2,False)
'Or, to disable output if Windspeed, Slr, or Temp value is invalid use:
'ETsz (Temp,RH,WindSpeed,SlrMj,Longitude,Latitude,Altitude,Zw,0,FP2,WindSpeed = 0 or SlrMj=NAN or Temp <=40)
Fieldnames ("ETGrass,SolarRadCalc")
EndTable

BeginProg
Zw = 3
Scan (1,Sec,0,0)

'Temp & RH
Therm107(Temp,1,1,Vx1,0,15000,1.0,0.0)

PortSet(C1,1)
Delay(0,150,mSec)
VoltSE(RH,1,mv5000C,1,0,0,15000,0.1,0)

PortSet(C1,0)
If RH>100 And RH<108 Then RH=100 'set RH to 100 if between 100 & 108

'Wind speed measurement
PulseCount (WindSpeed,1,P1,1,1,0.7990,0.2811)

'Pyranometer measurements SlrMJ and SlrkW
VoltSE(SlrkW,1,mv5000C,1,1,0,15000,1,0)

If SlrkW<0 Then SlrkW=0
SlrMJ=SlrkW*0.000025
SlrkW=SlrkW*0.005

CallTable MetData

NextScan
EndProg
```

## Remarks

This instruction follows the implementation in "The ASCE Standardized Reference Evapotranspiration Equation" published by the ASCE in 2005 and edited by Richard G. Allen and colleagues. Specifically, it uses the calculations for hourly intervals with modifications for tall reference wind from Appendix B and modifications to clear-sky short wave radiation in Appendix D. The user can choose from a short reference or tall reference crop calculation.

**NOTE:** This instruction uses an algorithm which is intended for hourly output. Use at other output intervals will lead to invalid results. Note that the datalogger's clock must be set to Standard time, not daylight saving time.

There are two outputs: water loss in millimeters and the calculated clear sky solar radiation value for the site in MJ m-2. Calculated clear sky solar radiation (Rso) is defined as the amount of solar radiation that would be received at the weather measurement site under conditions of clear sky (i.e., cloud-free). Both values are the totals for the output interval of the data table in which the instruction is used (normally hourly). Calculated clear sky solar radiation is returned to help the user determine if the solar radiation sensor is functioning properly.

Crop evapotranspiration (ETc) can be calculated by post-processing the ETo estimates as follows:

ETc = Kc \* ETo

where

ETc = crop evapotranspiration

Kc = crop coefficient

and ETo = reference crop evapotranspiration

A variety of sources exist for obtaining crop coefficients for specific crops and crop growth stages.

If the DisableVar is true for the entire output interval,NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is stored. Also, If a NAN is included in the values being processed, NAN is stored. Note that since there is no such thing as NAN for integers, values that are converted from float to integer are expressed in data tables as the most negative number for a given data type. For example, the most negative number of data type FP2 is -7999, so NAN for FP2 data will appear in a data table as -7999. If the data type is Long, NAN will appear in the data table as -2147483648.

Call the output table conditionally (for example, do not call the table if a variable = NAN) to keep NANs from affecting the other good values.

The variable names ETos and ETrs are used for the short and long reference calculations, respectively, with a default unit of mm assigned. Default unit for Rso is MJ/m^2.

## Parameters

# Temp (Temperature)

The variable in which the value for temperature is stored. The value must be in degrees C.

Type: Variable

While not critical for the ETsz instruction, temperature is usually measured at a height of 1.5 to 2.5 meters.

# RH (Relative Humidity)

The variable in which the value for relative humidity is stored. The value must be in percent.

Type: Variable

# uZ (Wind Speed)

The uZ parameter is the variable in which the wind speed measurement is stored that will be used for the ETsz calculation. The wind speed measurement must be in meters per second. The wind sensor height entered below (Zw) is used to derive the wind at 2 meters needed for the ET calculation.

Type: Variable

# Rs (Solar Radiation)

The variable in which the solar radiation measurement is stored that will be used for the ETsz calculation. The solar radiation measurement must be scaled to give total MegaJoules per meter squared (MJ m-2) received during the period between calling the table with the ETsz instruction in it (this is normally the scan interval). If the sensor is scaled to output flux; for example Wm-2, this would need to be multiplied by the scan interval in seconds and a multiplier applied to convert to MJ; for example, 10-6to convert from W.

Type: Variable

# Longitude

A constant or variable that provides the longitude for the datalogger station, expressed in decimal degrees west of the Greenwich meridian. For longitudes measured going east from the Greenwich meridian, a corrected value (360 EastDegrees) can be used.

Type: Constant or variable

# Latitude

A constant or variable that provides the latitude for the datalogger station. The range for Latitude is 0 to 90 (positive value for Northern hemisphere and negative value for Southern hemisphere).

Type: Constant or variable

# Altitude

A constant or variable that provides the altitude above sea level for the datalogger station. This value must be in meters.

Type: Constant or variable

# Zw (Wind Speed Sensor Height)

The height of the wind speed sensor in meters.

Type: Variable or constant

# Sz (Crop Reference)

Used to select which crop reference to use for the ETsz calculation. Enter 0 for short reference calculation (similar to clipped grass) or 1 for tall reference calculation (similar to full-cover alfalfa). Right click the parameter to display a list.

Type: Integer

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
