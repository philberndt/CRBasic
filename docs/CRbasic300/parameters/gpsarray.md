# GPSArray (GPS Array)

The variable in which to store the information returned by the GPS. Fifteen values are returned. If this array is not dimensioned to 15, values will be stored to fill the array and no error will be returned. If no values are available, NAN will be returned. The values in this array are updated only when the GPS instruction is executed.

**NOTE:** Beginning with operating system8, the GPSArray may be declared as type Double to support higher-precision latitude and longitude.

The following values are returned by the GPS:

- **Array(1)**: Latitude, degrees

- ** Array(2)**: Latitude, minutes

- ** Array(3)**: Longitude, degrees

- ** Array(4)**: Longitude, minutes

- ** Array(5)**: Speed over ground, knots

- ** Array(6)**: Course over ground, degrees

- ** Array(7)**: Magnetic variation (positive = East, negative = West)

- ** Array(8)**: Fix Quality (0 = invalid, 1 = GPS, 2 = differential GPS, 6 = estimated)

- ** Array(9)**: Number of Satellites

- ** Array(10)**: Altitude, meters

- ** Array(11)**: Time into the current second (in microseconds) when the PPS low to high transition occurred. Once the clock is set a fixed value of 990000 will be returned.

- ** Array(12)**: Time since the datalogger last saw a valid GPRMC sentence, timed at the instant the GPS instruction runs. Typically this value will be about one second. If this value is seen to be consistently above one second and/or increasing this indicates the GPS unit has lost sight of enough satellites to give a good output or the GPS unit is disconnected or powered off.

- ** Array(13)**: GPSReady. Indicates whether the datalogger has seen a number of good GPRMC strings, all received within a reasonable time of the PPS signal. A value of ten indicates 10 good responses have been received. Only when the counter is at ten will the datalogger use the GPS time to set the datalogger clock. If a GPRMC string comes out in the wrong time frame or if it is invalid in any way, the GPS Ready value is reset to zero.

- ** Array(14)**: Maximum time adjustment since the datalogger program started, milliseconds (10 msec resolution). Can be set to 0 manually or under program control.

- ** Array(15)**: Number of times clock has been changed since the datalogger program started. This can be set to 0 manually or under program control.

Data from other NMEA sentences can be stored in a numeric format. For each sentence data is stored in the array as follows:

1. The number of values in the sentence.
2. A simple checksum of the characters in the sentence's name, not counting the '$'. This is the sum of the 8 bit ASCII codes.
3. The comma separated values in the sentence. If the field is non-numeric, a similar numeric checksum for the variable string as in (2) is stored. For a single character field this will be its ASCII value. If the field is vacant; i.e., back to back commas,NAN

   ** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

   is stored.

Steps 1..3 are repeated for as many different sentences that are seen by the datalogger. The GPSArray must be dimensioned large enough to store all the variables for all the sentences you need to store. The order in which the data is stored for the different NMEA sentences follows the rules for the ordering of the NMEA strings.

These values are stored in the GPS array when the GPS instruction runs, without risk of corruption caused by new data coming in when the instruction runs.

Type: Variable
