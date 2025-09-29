# CellGNSS (Global navigation satellite system for CR350-CELL230)

The CellGNSS instruction can be used in a CR350-CELL230 to provide GNSS information, such as fix location, latitude, longitude, speed over ground, and number of satellites.

**Caution:** Execution of the CellGNSS instruction will disable the cell network until a GPS fix is acquired or the max-wait-time parameter has been satisfied.

## Syntax

CellGNSS(GNSSArray,GNSSTime,[MaxWaitTime](../parameters/gnss-max-wait-time.md))

### Example #1

'The following program provides an example of using the CellGNSS() instruction with a CR350-CELL230.

```
Public gnss_data(8)
Alias gnss_data(1) = fix_quality'0 = No Fix,timeout, 2 = 2D-fix, 3 = 3D-fix
Alias gnss_data(2) = latitude'(-)dd.ddddd,(-)ddd.ddddd
Alias gnss_data(3) = longitude'(-)dd.ddddd,(-)ddd.ddddd
Alias gnss_data(4) = elevation'Elevation (meters)
Alias gnss_data(5) = speed'Speed over ground (Km/h (SPKM)
Alias gnss_data(6) = course'Course over ground, degrees (COG)
Alias gnss_data(7) = horizontal_precision'(HDOP): 0.5-99.9
Alias gnss_data(8) = nmbr_satellites
```

Public gnss_time(9)
Alias gnss_time(1) = year
Alias gnss_time(2) = month
Alias gnss_time(3) = \_day'day is a predefined constant; use \_day instead
Alias gnss_time(4) = hours
Alias gnss_time(5) = minutes
Alias gnss_time(6) = seconds
Alias gnss_time(7) = microseconds'CELL230 returns 0
Alias gnss_time(8) = day_of_week'CELL230 returns 0
Alias gnss_time(9) = day_of_year'CELL230 returns 0

```
BeginProg
'Use SetSetting prior to scan To enable CellGNSS
SetSetting ("CellGNSS",1)
```

Scan (10,Min,0,0)

```
'CellGNSS(GNSS_Array, GNSSTime, MaxWaitTime (ms)
```

CEllGNSS(gnss_data(),gnss_time(),60000)'max wait time = 60 seconds
NextScan
EndProg

```

## Remarks


**NOTE:**In addition to the CellGNSS instruction, the data logger also features a CELLGNSS setting which is used to enable or disable CellGNSS functionality.

Upon detecting a CELL230, GNSS location details including GNSSFix, GNSSLon, GNSSLat, GNSSAlt, GNSSNumSat, GNSSTime, and GNSSSuccessCnt are presented in the data loggers Status table. Programmatic access to CellGNSS location details can be achieved in two ways: 1) using the CellGNSS instruction for comprehensive GNSS location data, or 2) employing the`tablename.fieldname`syntax for specific data fields from the data loggers Status table. The CellGNSS instruction offers a straightforward method to obtain all GNSS details, while the`tablename.fieldname`syntax is useful for accessing individual GNSS fields, such as`Status.GNSSxxx`, where xxx indicates a specific GNSS field.

If the CELLGNSS setting in the data logger is disabled, GNSSFix will display "Disabled", GNSSLon and GNSSLat will show "NAN", and GNSSAlt, GNSSNumSat, GNSSTime, and GNSSSuccessCnt will all be zero. The CELLGNSS setting can be toggled programmatically using the SetSetting() instruction, with SetSetting("CellGNSS",1) to enable and SetSetting("CellGNSS",0) to disable.

**Caution:** Toggling the CELLGNSS setting from enabled to disabled, or vice versa, will reboot the modem which will need to re-establish a connection to the cell network.


## Parameters



# GNSSArray (GNSS Array)


The variable array in which to store the information returned by the CellGNSS instruction. This array returns 8 values and must be dimensioned to 8. If no values are available, 0 will be returned. The values in this array are updated each time a fix location is recorded. In the event of a timeout, the fix parameter will be set to 0 and these values will not be updated.

The following values are returned by the CellGNSS instruction:

- **Array(1)**: Fix quality (0 = no fix - timeout; 2 = 2D fix; 3 = 3D fix)

- ** Array(2)**: Latitude (-)dd.ddddd,(-)ddd.ddddd

- ** Array(3)**: Longitude (-)dd.ddddd,(-)ddd.ddddd

- ** Array(4)**: Elevation, meters

- ** Array(5)**: Speed over ground, Km/h (SPKM)

- ** Array(6)**: Course over ground, degrees (COG)

- ** Array(7)**: Horizontal precision (HDOP): 0.5-99.9

- ** Array(8)**: Number of satellites

Type: Variable


# GNSSTime (GNSS Time Array)


The variable array in which to store the time in Coordinated Universal Time (UTC) returned by the CellGNSS instruction. This array returns 9 values and must be dimensioned to 9. If no values are available, 0 will be returned. The values in this array are updated each time a fix location is recorded. In the event of a timeout, the fix parameter will be set to 0 and these values will not be updated.

The following values are returned by the CellGNSS instruction:

- ** Array(1)**: Year

- ** Array(2)**: Month

- ** Array(3)**: Day

- ** Array(4)**: Hours

- ** Array(5)**: Minutes

- ** Array(6)**: Seconds

- ** Array(7)**: Microseconds

- ** Array(8)**: Day of week (Sunday = 1; (1..7)

- ** Array(9)**: Day of year (1..366)

** NOTE:**Because the CELL230 does not report microseconds, day of week, or day of year, 0 is returned for array elements 7 to 9.

Type: Variable


# MaxWaitTime (Max Wait Time)


The maximum amount of time, in milliseconds, the CellGNSS instruction will wait for a location fix before timing out. Generally, it takes 25 to 90 seconds (25000 to 90000 ms) to get a location fix, but in some instances, it can take several minutes.

Type: Constant or variable expression that evaluates as a constant
```
