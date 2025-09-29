# GOESGPS (Store GOES GPS Data)

The GOESGPS instruction is used to store GPS data from the satellite into two variable arrays. This instruction works with the TX321, TX320, and TX312 satellite transmitters,_but is not compatible with the TX325._

## Syntax

GOESGPS(GoesArray1(6),GoesArray2(7))

In the following program, GPS data from the GOES transmitter is stored into an array. The variables of that array are Aliased [(seeAlias)](alias.md), for identification of the returned data values.

```
Public GOESResult1(6)
Alias GOESResult1(1) = ResultCode
Alias GOESResult1(2) = Time
Alias GOESResult1(3) = Latitude
Alias GOESResult1(4) = Longitude
Alias GOESResult1(5) = Elevation
Alias GOESResult1(6) = MagVariation

Public GOESResult2(7)
Alias GOESResult2(1) = Year
Alias GOESResult2(2) = Month
Alias GOESResult2(3) = DOM'Day of month (Day cannot be used It is a predefined constant in CRBasic.)
Alias GOESResult2(4) = Hour
Alias GOESResult2(5) = Minutes
Alias GOESResult2(6) = Seconds
Alias GOESResult2(7) = Microsec

BeginProg
Scan (1,Sec,3,0)
GOESGPS(GOESResult1(), GOESResult2())
NextScan
EndProg
```

## Remarks

The GOESGPS instruction returns two arrays.

## Parameters

# GOESArray1 (GOES Array Result 1)

An array that holds a result code indicating the success of the instruction, followed by global positioning information.

The result codes are as follows:

| Code | Description                                                                     |
| ---- | ------------------------------------------------------------------------------- |
| 0    | Command executed successfully.                                                  |
| 2    | Timed out waiting for STX character from transmitter after SDC addressing.      |
| 3    | Wrong character received after SDC addressing                                   |
| 4    | Something other than ACK returned when select data buffer command was executed. |
| 5    | Timed out waiting for ACK.                                                      |
| 6    | Port not available; GOES not attached.                                          |
| 7    | ACK not returned following data append or overwrite command.                    |

The GPS data values are as follows:

| Value              | Description                                   |
| ------------------ | --------------------------------------------- |
| Time               | Seconds since January 1, 2000                 |
| Latitude           | Fractional degrees; 100 nanodegree resolution |
| Longitude          | Fractional degrees; 100 nanodegree resolution |
| Elevation          | Signed 32-bit number, in centimeters          |
| Magnetic Variation | Fractional degrees; 1 millidegree resolution  |

Type: Variable array dimensioned to 6

# GOESArray2 (GOES Array Result 2)

The second array, which must be dimensioned to 7, holds the following time values: year, month, day hour (GMT), minute seconds, microseconds.
