# GPS (Synchronize Clock with a GPS Device)

The GPS instruction is used to keep the datalogger clock synchronized with a GPS device.This instruction is available in Operating System 6 and newer.

**NOTE:** This instruction disables low-power (idle)mode.

## Syntax

GPS(GPSArray,ComPort,PPS_Port,TimeOffset,MaxTimeDiff,NMEAStrings[optional],GPSBaudrate[optional])

### Example #1

The following wiring and short program provide an example of using the GPS instruction with the Garmin GPS16-HVS.

This program was written for aCR300, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
'Program the GPS16-HVS to use 38.4 kbaud, no parity, 8 data bits, and 1 stop bit
'*** Wiring ***
'CONTROL PORTS
'C7 GPS16-HVS pulse per second (yellow)
'C8 GPS16-HVS RS-232 TxD (green)
'G GPS16-HVS power control (black)
'POWER OUT
'12V GPS16-HVS power (red)
'G GPS16-HVS power and RS-232 signal reference (orange)

PipeLineMode

Const LOCAL_TIME_OFFSET = -6'Local time offset relative to UTC time

Dim nmea_sentence(2) As String *100
Public gps_data(15)
Alias gps_data(1) = latitude_a'Degrees latitude (+ = North; - = South)
Alias gps_data(2) = latitude_b'Minutes latitude
Alias gps_data(3) = longitude_a'Degress longitude (+ = East; - = West)
Alias gps_data(4) = longitude_b'Minutes longitude
Alias gps_data(5) = speed'Speed
Alias gps_data(6) = course'Course over ground
Alias gps_data(7) = magnetic_variation'Magnetic variation from true north (+ = East; - = West)
Alias gps_data(8) = fix_quality'GPS fix quality: 0 = invalid, 1 = GPS, 2 = differential GPS, 6 = estimated
Alias gps_data(9) = nmbr_satellites'Number of satellites used for fix
Alias gps_data(10) = altitude'Antenna altitude
Alias gps_data(11) = pps'usec into sec of system clock when PPS rising edge occurs, typically 990,000 once synced
Alias gps_data(12) = dt_since_gprmc'Time since last GPRMC string, normally less than 1 second
Alias gps_data(13) = gps_ready'Counts from 0 to 10, 10 = ready
Alias gps_data(14) = max_clock_change'Maximum value the clock was changed in msec
Alias gps_data(15) = nmbr_clock_change'Number of times the clock was changed

'Define Units to be used in data file header
Units latitude_a = degrees
Units latitude_b = minutes
Units longitude_a = degrees
Units longitude_b = minutes
Units speed = m/s
Units course = degrees
Units magnetic_variation = unitless
Units fix_quality = unitless
Units nmbr_satellites = unitless
Units altitude = m
Units pps = ms
Units dt_since_gprmc = s
Units gps_ready = unitless
Units max_clock_change = ms
Units nmbr_clock_change = samples

BeginProg
'Use SetStatus prior to scan if baud rate needs to be changed for device
Scan (1,Sec,0,0)
GPS(latitude_a,COMC1_Rx,C2,LOCAL_TIME_OFFSET*3600,100,nmea_sentence(1))
NextScan
EndProg
```

### Example #2

The following program demonstrates reading in NMEA sentences in addition to GPRMC and GPPA

```
'Locations for the extra NMEA sentences should be set apart by initializing the string variables

Public NMEAStrings(4) As String *100 = {"$GPRMC","$GPGGA","$GPVTG","$GPGSA"}
Dim GPS_Data(44)'The data array needs to be large enough to hold all the extra values,
'you can swithc it to Pulbic for troubleshooting
Public Latitude_a'Degrees latitude (+ = North; - = South)
Public Latitude_b'Minutes latitude
Public Longitude_a'Degrees longitude (+ = East; - = West)
Public Longitude_b'Minutes longitude
Public Speed'Speed
Public Course'Course over ground
Public Magnetic_Variation'Magnetic variation from true north (+ = East; - = West)
Public Fix_quality'GPS fix quality: 0 = invalid, 1 = GPS, 2 = differential GPS, 6 = estimated
Public nmbr_satellites'Number of satellites used for fix
Public Altitude'Antenna altitude
Public pps'usec into sec of system clock when PPS rising edge occurs, typically 990,000 once synced
Public dt_since_gprmc'Time since last GPRMC string, normally less than 1 second
Public gps_ready'Counts from 0 to 10, 10 = ready
Public max_clock_change'Maximum value the clock was changed in msec
Public nmbr_clock_change'Number of times the clock was changed

'Define Units to be used in data file header
Units Latitude_a = degrees
Units Latitude_b = minutes
Units Longitude_a = degrees
Units Longitude_b = minutes
Units Speed = knots
Units Course = degrees
Units Magnetic_variation = unitless
Units Fix_quality = unitless
Units nmbr_satellites = unitless
Units Altitude = m
Units pps = ms
Units dt_since_gprmc = s
Units gps_ready = unitless
Units max_clock_change = ms
Units nmbr_clock_change = samples

Dim tempValue As Long
Public GPVTG_start As Long
Public GPVTG_count As Long
Public GPVTG_Data(8)
Public GPGSA_start As Long
Public GPGSA_count As Long
Public GPGSA_Data(17)

DataTable (FifteenMinute,1,-1)
DataInterval (0,15,Min,10)
Sample (8,GPVTG_Data,IEEE4)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)

GPS(GPS_Data(),COMC1_Rx,C2,0,100,NMEAStrings)

Latitude_a = GPS_Data(1)
Latitude_b = GPS_Data(2)
Longitude_a = GPS_Data(3)
Longitude_b = GPS_Data(4)
Speed = GPS_Data(5)
Course = GPS_Data(6)
Magnetic_variation = GPS_Data(7)
Fix_quality = GPS_Data(8)
nmbr_satellites = GPS_Data(9)
Altitude = GPS_Data(10)
pps = GPS_Data(11)
dt_since_gprmc = GPS_Data(12)
gps_ready = GPS_Data(13)
max_clock_change = GPS_Data(14)
nmbr_clock_change = GPS_Data(15)

'If an extra sentence is not present, count of 2 and checksum will still output in its place
GPVTG_start = 16'The normal output of the GPS instruction has a start of 1 and a total of 15 values
GPVTG_count = GPS_Data(GPVTG_start)'The total number of values output related to the sentence
tempValue = GPVTG_count - 2'The first two values are the count and the checksum
Move(GPVTG_Data(),tempValue,GPS_Data(GPVTG_start + 2),tempValue)'Copy the proper values into their separate array

GPGSA_start = GPVTG_start + GPVTG_count'Offset by the number of previous data values
GPGSA_count = GPS_Data(GPGSA_start)'The total number of values outpu related to the sentence
tempValue = GPGSA_count - 2'The first two values are the count and the checksum
Move(GPGSA_Data(),tempvalue,GPS_Data(GPGSA_start + 2),tempValue)'Copy the proper values into their seperate array

CallTable FifteenMinute
NextScan
EndProg
```

## Remarks

This instruction is used alongwith a GPS deviceto set the datalogger's clock. This instruction will also provide information such as location (latitude/longitude) and speed, and store NMEA sentences from the GPS device.

The resolution of accuracy for the clock set is3 msec. The clock set relies on information from the GPRMC sentence. If this sentence is not returned a clock set will not occur.

If the datalogger is programmed to use the PPS signal for synchronization, the datalogger sets its clock approximately 200 microseconds behind the GPS clock (as indicated by the PPS signal). This means that the first measurement in the main scan will start about 300 microseconds behind GPS time. Synchronization of measurements between dataloggers of the same speed is typically +/- 1 msec.

By default, the instruction expects the GPS unit to be set up at 38400 baud, outputting the GPRMC and GPGGA sentences once per second. The datalogger expects the start of the second to coincide with the rising edge of the PPS signal. If there is no PPS signal or if the required sentences come out at less than once per second the datalogger will not update its clock. The datalogger will wait on 10 good responses from the GPS before using the data stream.

GPS units with lower baud rates can be used with this instruction by specifying the baudrate with the optional Baudrate parameter. Baud rates below 2400 bps will not work as the GPS unit will not be able to transmit the two GPS sentences once per second reliably. Similar problems can be encountered even at higher baud rates if too many optional GPS strings are selected to be output.

## Parameters

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

# ComPort (Communication Port)

The ComPort parameter is the port to which the GPS device is attached and that will be used to read in the NMEA sentences. Valid options are:

- ComRS232_Rx

- ComC1_Rx

- ComC2_Rx.

These are predefined constants selected via the drop-down list for this parameter.

Type: Constant

# PPS_Port (PPS Port)

Specifies the port used to read the PPS signal. If no PPS signal is to be read, enter 0 to disable

this parameter. Valid port options are:

- 0 - No PPS signal

- C1

- C2

- SE1

- SE2

- SE3

These are predefined constants.

Type: Constant

# TimeOffset (Time Offset)

The local time offset, in seconds, from UTC. The TimeOffset parameter is ignored if the "UTC Offset" setting in the datalogger is a value other than -1 (disabled). In other words, the UTCOffset setting in the datalogger overrides this instruction parameter. The UTCOffset setting can be found in the Settings table. For more information, seeSettings Available Using SetSetting

** NOTE:**For the GPS() instruction, in order to use GPS coordinates without setting the datalogger clock, set the TimeOffset parameter to -1.

Type: Constant

# MaxTimeDiff (Maximum Difference in Time)

The maximum difference in time (in milliseconds) between the datalogger clock and the GPS clock that will be tolerated before the clock is changed. If a negative value is entered, the clock will not be changed.

Type: Constant

## Optional Parameters

# NMEAStrings (NMEA Sentences)

The string array that holds the NMEA sentences. If it exists, the GPRMC sentence will reside in NMEAStrings(1), and the GPGGA sentence will reside in NMEAStrings(2). Any other sentences will reside in subsequent indexes into the array (on a first-in basis). Once an index in the array is used to store a particular sentence, that sentence will always be stored in that location when updates to the sentence are received.

The NMEA strings are updated every time a new sentence comes in as a background task, so the strings can be seen to change even if the GPS instruction is called infrequently. These strings are mainly provided for diagnostic purposes as the string can be overwritten by the background task midway through the program code accessing the string. Access to data in the standard or user configured additional messages should be using the GPSArray (see below).

Beyond the GPRMC and GPGGA sentences, you can determine the order the sentences appear in the NMEA strings by initializing the required NMEAstring to the strings name at the start of the program; for example NMEAStrings(3)= $GPVTG , otherwise the data is written in the order seen after the program starts to run.

Type: Variable Array Declared as a String

# GPSBaudrate (GPS Baud Rate)

This is an optional parameter that may be used to specify abaud rate

** Note:**The rate at which data is transmitted.

.

By default, the instruction expects the GPS unit to be set up at 38400 baud, outputting the GPRMC and GPGGA sentences once per second. The datalogger expects the start of the second to coincide with the rising edge of the PPS signal. If there is no PPS signal or if the required sentences come out at less than once per second the datalogger will not update its clock. The datalogger will wait on 10 good responses from the GPS before using the data stream.

GPS units with lower baud rates can be used with this instruction by specifying the baud rate with the optional GPSBaudrate parameter. Baud rates below 2400 bps will not work as the GPS unit will not be able to transmit the two GPS sentences once per second reliably. Similar problems can be encountered even at higher baud rates if too many optional GPS strings are selected to be output.

Type: Constant
