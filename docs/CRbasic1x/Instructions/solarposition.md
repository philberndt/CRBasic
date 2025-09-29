# SolarPosition (Solar Position)

The SolarPosition instruction is used to calculate solar position from program inputs.

## Syntax

SolarPosition(Dest,TimeArray,TimeOffset,Latitude,Longitude,Altitude,Pressure,AirTemp)

The following is aCR1000X Seriesprogram example using the SolarPosition instruction. This same program can be used for other dataloggers with little or no modification (check parameters that may vary for dataloggers, such as voltage ranges).

The program also provides the necessary calculations for Local Solar Noon, Sunrise Time, and Sunset Time.

Time Offset (from GMT), Latitude, Longitude, and Altitude have been entered as part of a constant table so that they can be changed on the fly for program testing.

```
Public Dest(5)
Public pressure = -1'-1 may be used if a pressure measurement is unavailable
Public airtemp, Reftemp
Public DeclinationDeg'used to convert the declination from radians to degrees
Public HrAngleDeg, Local_SolarNoon
Public SunRise, SunSet, SunHourDuration
Dim Time(9)

Alias Dest(1) = SolarAzimuth
Alias Dest(2) = SunElevation
Alias Dest(3) = HourAngle
Alias Dest(4) = Declination
Alias Dest(5) = AirMass

Units SolarAzimuth= deg
Units SunElevation= deg
Units Declination= Rad
Units HourAngle= Rad
'Airmass has no units as it is a ratio

'Constant table can be used to change UTCOffset, Lat, Long & Altitude in the field
'Enter offset in hours, Lat/Long in deg, and altitude in (meters)
'Conversions done within SolarPosition instruction to meet parameter requirements:
'Offset hours to seconds
'Altitude to meters
ConstTable
Const UTC_Offset=-6'must be in units of seconds when used in SolarPosition
Const Latitude = 41.7658'Latitude nearCampbell Scientific
Const Longitude = -111.8549'Longitude nearCampbell Scientific
Const Altitude = 1371.6'must be in units of meters when used in SolarPosition
EndConstTable

BeginProg
Scan (1,Sec,3,0)
RealTime (Time())
PanelTemp (RefTemp,15000)
TCDiff (airtemp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

'The datalogger panel temperature may be used instead of an ambient air
'temperature measurement. However, Accuracy will likely be affected.
SolarPosition(Dest,Time,UTC_Offset*3600,Latitude,Longitude,Altitude,pressure,airtemp)
DeclinationDeg=Declination*57.2957795'Converts Declination to degrees
HrAngleDeg = HourAngle*57.2957795
Local_SolarNoon = Time(4) +Time(5)/60+Time(6)/3600 - HrAngleDeg/15
SunSet = Local_SolarNoon +57.2957795*(ACOS(-TAN(Declination)*TAN(Latitude/57.2957795)))/15
SunRise = Local_SolarNoon - 57.2957795*(ACOS(-TAN(Declination)*TAN(Latitude/57.2957795)))/15
SunHourDuration = SunSet-SunRise
```

NextScan
EndProg

```

## Remarks


The SolarPosition instruction makes use of the Solpos algorithm developed by the National Renewable Energy Laboratory (NREL). The instruction has a minimum uncertainty of 0.01 degrees in both the zenith and azimuth angles. This level of accuracy is more than adequate for most engineering applications.

The instruction returns five elements into the Destination (Dest) array:

- Dest(1) = SolarAzimuth (degrees from North; N = 0, E = 90, S = 180, W = 270)

- Dest(2) = SunElevation (degrees above the horizon)

- Dest(3) = HourAngle (Radians West from solar noon). Converts local solar time into an angular displacement for the sun's movement across the sky.

- Dest(4) = Declination (Radians North or South). Angle between the equator and the location of the sun.

- Dest(5) = AirMass (air mass coefficient). Ratio of the path length of the current solar position to the path length at solar noon. Calculated via Kasten and Young s method 1989. If a pressure measurement is possible, a correction is applied and will refer to the airmass as an absolute airmass. If pressure is not used, airmass may be referred to as a relative airmass.

The output from SolarPosition can also be used to calculate the following parameters for the site:

- Local Solar Noon time

- Time of sunrise

- Time of Sunset

The HourAngle is a negative number in the morning before solar noon; it is positive in the afternoon after solar noon. HourAngle can be used to calculate Local Solar Noon as well as sunrise and sunset time for the location. The example program includes these calculations.


## Parameters



# Dest (Destination)


The variable array in which the results of the instruction will be written. Dest must be dimensioned to 5 or a compilation error will occur. The following 5 values are returned:

- Dest(1) = SolarAzimuth (degrees from North; N = 0, E = 90, S = 180, W = 270)

- Dest(2) = SunElevation (degrees above the horizon; 90 degrees corresponds to solar noon)

- Dest(3) = HourAngle (Radians West from solar noon). Converts local solar time into an angular displacement for the sun's movement across the sky.

- Dest(4) =Declination (Radians North or South). Angle between the equator and the location of the sun.

- Dest(5) = AirMass (air mass coefficient). Ratio of the path length of the current solar position to the path length at solar noon. Calculated via Kasten and Young s method 1989. If a pressure measurement is possible, a correction will be applied and will refer to the airmass as an absolute airmass. If pressure is not used, airmass may be referred to as a relative airmass.


# TimeArray (Time Array)


A variable array dimensioned to 9 that holds the following values: (1) year, (2) month, (3) day of month, (4) hour of day, (5) minutes, (6) seconds, (7) microseconds, (8) day of week (1-7; Sunday = 1), and (9) day of year. These values can be derived from the [RealTime](realtime.md) instruction. If TimeArray is not dimensioned to 9, a compilation error will occur.

Type: Variable array


# TimeOffset (Time Offset)


The local time offset, in seconds, from UTC. The TimeOffset parameter is ignored if the "UTC Offset" setting in the datalogger is a value other than -1 (disabled). In other words, the UTCOffset setting in the datalogger overrides this instruction parameter. The UTCOffset setting can be found in the Settings table. For more information, seeSettings Available Using SetSetting

**NOTE:** For the GPS() instruction, in order to use GPS coordinates without setting the datalogger clock, set the TimeOffset parameter to -1.

Type: Constant


# Latitude


A constant or variable that provides the latitude for the datalogger station. The range for Latitude is 0 to 90 (positive value for Northern hemisphere and negative value for Southern hemisphere).

Type: Constant or variable


# Longitude


A constant or variable that provides the longitude for the datalogger station, expressed in decimal degrees east of the Greenwich meridian. For longitudes measured going west from the Greenwich meridian, a negative value can be entered or a corrected value (360 WestDegrees) can be used. For example, Tokyo, Japan has a longitude of 220.3 when measured going west. This could be entered as -220.3, or the corrected longitude value of 139.7 degrees East could be used.

Type: Constant or variable


# Altitude


A constant or variable that provides the altitude above sea level for the datalogger station. This value must be in meters.

Type: Constant or variable


# Pressure (Atmospheric Pressure)


The variable that holds the local annual average barometric pressure (not current measurement) in millibars, that will be used in calculating the solar position. If Pressure is defined as -1, AirTemp will be used to calculate an estimate of barometric pressure.

Type: Variable or constant


# AirTemp (Air Temperature)


The variable that holds a temperature measurement in degrees C that will be used in calcuating the solar position.

NANs may occur in the output array for Zenith angles if a temperature measurement is not used. If temperature is unavailable, consider using the datalogger's panel temperature (though this may affect accuracy of the instruction).

Type: Variable
```
