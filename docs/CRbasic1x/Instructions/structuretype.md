# StructureType/EndStructureType (Define Structure)

The StructureType instruction is used to define a structure. The EndStructureType statement designates the end of the structure definition. Structures are used to organize variables and display data in a structured manner and can significantly shorten program code, especially for instructions that output an array of values, such as AVW200(), GPS(), SDI12Recorder() and more. For example, a single StructureType may be used to organize and display data for multiple vibrating wire sensors or many SDI-12 sensors without creating aliases for each sensor.

Another advantage of structures is the ability to define different variable types within the same array. For example, with GPS(), latitude minutes and longitude minutes may be double precision while other values in the output array are single precision.

Structures may also be used as the destination variable for measurement instructions. Inside the destination structure, you can declare an array of multipliers and offsets for each measurement that are easy to view and adjust in the field.

## Syntax

StructureTypeName

structure list

EndStructureType

### Example #1

```
'This program demonstrates using a structure to shorten the code required for measuring 3
'vibrating wire sensors with one multiplexer on an AVW200.

'Declare a Structure Type named VWSensor
StructureTypeVWSensor
Frequency As Float
Units Frequency = Hz
Amplitude As Float
Units Amplitude = mV RMS
SNR As Float
NoiseFreq As Float
Units NoiseFreq = Hz
Decay As Float
Therm As Float
Units Therm = Ohms
ReadOnly Frequency,Amplitude,SNR,NoiseFreq,Decay,Therm
EndStructureType

Const NumberOfSensors As Long = 3'Number of vibrating wire sensors to measure

DataTable (AVWTable,True,500)'Save output from Piezometer structure to a table
'for output processing
DataInterval (0,60,Sec,10)
Sample (3,Piezometer,VWSensor)
Fieldnames("sensor1,sensor2,sensor3")
Average (3,Piezometer,VWSensor,False)
Fieldnames("sensor1_avg,sensor_avg2,sensor3_avg")
Minimum (3,Piezometer,VWSensor,False,False)
Fieldnames("sensor1_min,sensor2_min,sensor3_min")
Maximum (3,Piezometer,VWSensor,False,False)
Fieldnames("sensor1_max,sensor2_max,sensor3_max")
EndTable

'Declare Public Variables
Public AVWResult
Public Piezometer(NumberOfSensors) As VWSensor'Declare a public structure named Piezometer

'Using a structure negates the need for aliasing the destination variable because
'each sensor number is a structure index which displays as a row in a structure table
'Alias Dest(1,1)= Frequency1
'Alias Dest(1,2)= Amplitude1
'Alias Dest(1,3)= SNR1
'Alias Dest(1,4)= NoiseFreq1
'Alias Dest(1,5)= DecayRatio1
'Alias Dest(1,6)= Thermistor1
'Alias Dest(2,1)= Frequency2
'Alias Dest(2,2)= Amplitude2
'Alias Dest(2,3)= SNR2
'Alias Dest(2,4)= NoiseFreq2
'Alias Dest(2,5)= DecayRatio2
'Alias Dest(2,6)= Thermistor2

'Thermistor coefficients
Const A = 1.4051E-3
Const B = 2.369E-4
Const C = 1.019E-7

'Main Program
BeginProg
Scan (6,Sec,3,0)'2 sec per sensor
'Use the Piezometer structure as the destination variable for the AVW200 instruction
AVW200 (AVWResult,ComRS232,200,200,Piezometer,1,1,3,300,6000,1,_60Hz,1,0)
CallTable AVWTable
NextScan
EndProg
```

### Example #2

```
'This program demonstrates using structure variables as the destination parameter
'for the SDI12Recorder instruction. In this example, 3 CS215 Temperature and
'Humidity Probes are used to measure temperature and humidity.

'Declare Public variables
Public BattV
Public RefTemp

StructureTypeTempRHSensor'Name and define the StructureType
Temp As Float'declare structure variables
RH As Float
ReadOnly Temp, RH'since these are measurement variables, best practice is to delcare these as ReadOnly
Units Temp = C'optional; declare units if desired
Units RH = %
EndStructureType

Const NumberOfSensors As Long =3
Public CS215(NumberOfSensors) As TempRHSensor

'Example for how to store data from all three sensors in one table.
DataTable (TempRH,True,1000)
DataInterval (0,10,Sec,10)
'Sampling an array of structures will generate duplicate field names
'This because each array element has the same structure list
'Use fieldnames to prevent duplicate field names in table
Sample (NumberOfSensors,CS215,TempRHSensor)
Fieldnames("Sensor1,Sensor2,Sensor3")
Minimum (NumberOfSensors,CS215,TempRHSensor,False,False)
FieldNames("Sensor1_min,Sensor2_min,Sensor3_min")
Maximum (NumberOfSensors,CS215,TempRHSensor,False,False)
FieldNames("Sensor1_max,Sensor2_max,Sensor3_max ")
EndTable

'Example for how to sample only the 1st element in the structure array
DataTable (Sensor1,True,10)
DataInterval (0,10,Sec,10)
Sample (1,CS215(1),TempRHSensor)
EndTable

'Example for how to sample only the 2nd element in the structure array
DataTable (Sensor2,True,10)
DataInterval (0,10,Sec,10)
Sample (1,CS215(2),TempRHSensor)
EndTable

'Example for how to sample only temperature from the 3 sensors
DataTable (Temponly,True,10)
DataInterval (0,10,Sec,10)
Sample (1,CS215(1).Temp,Float)
FieldNames("Temp1")
Sample (1,CS215(2).Temp,Float)
FieldNames("Temp2")
Sample (1,CS215(3).Temp,Float)
FieldNames("Temp3")
EndTable

BeginProg
Scan (5,Sec,3,0)

PanelTemp (RefTemp,15000)
Battery (BattV)
'6 values are written to CS215 = Temp and RH from 3 CS215s addresses 0,1,2
SDI12Recorder(CS215,C1,"012","M!",1,0)'2 values will be returned for each sensor; Temp and RH
CallTable TempRH
CallTable Sensor1
CallTable Sensor2
CallTable TempOnly

NextScan
EndProg
```

### Example #3

The following example program demonstrates using StructureType to declare latitude and longitude minutes as double-precision while other variables in the GPS output array are single-precision. Comport used isCOMC1.

```
PipeLineMode

Const LOCAL_TIME_OFFSET = -6'Local time offset relative to UTC time

StructureTypegps_t
LatitudeDegrees
LatitudeMinutes As Double
LongitudeDegrees
LongitudeMinutes As Double
Speed,Course,Magnetic_variation,Fix_quality,nmbr_satellites,Altitude,PPS
dt_since_gprmc,gps_ready,max_clock_change,nmbr_clock_change
Units LatitudeDegrees = degrees
Units LatitudeMinutes = minutes
Units LongitudeDegrees = degrees
Units LongitudeMinutes = minutes
Units Speed = m/s
Units Course = degrees
Units Magnetic_variation = unitless
Units Fix_quality = unitless
Units nmbr_satellites = unitless
Units Altitude = m
Units PPS = ms
Units dt_since_gprmc = s
Units gps_ready = unitless
Units max_clock_change = ms
Units nmbr_clock_change = samples
NMEASentences(2) As String * 110
EndStructureType

DataTable(LatLong,True,10)
'When a specific structure member is selected, match the Data Type to the structure member type
Sample(1,GPSData.LongitudeMinutes,Double)
Sample(1,GPSData.LatitudeMinutes,Double)
Sample(1,GPSData.Speed,FP2)
Sample(1,GPSData.Altitude,FP2)
EndTable

Public GPSData As gps_t

BeginProg
Scan (1,Sec,0,0)
'Use the GPSData structure as the desination variable
GPS (GPSData,ComC1,LOCAL_TIME_OFFSET*3600,100)
CallTable LatLong
NextScan
EndProg
```

## Remarks

Each StructureType is given a name. The StructureType name is limited to 20 characters and must start with an alphabetical character. Note that the StructureType name cannot be any of the predefined table names in the datalogger; that is, Public, Status, Settings, or DataTableInfo. Multiple structures may be declared as the same StructureType.

The StructureList consists of a comma-delimited or a line-separated list of variables or variable arrays that will be grouped together into one StructureType. Variable types supported are Float, Long, String, Boolean, and Double. If a variable within the StructureType is to be used as the destination for a measurement instruction, the variable should be declared as type Float and as read only, with the [ReadOnly()](readonly.md) instruction.

After a StructureType is defined, it is assigned to a structure. Structures may be declared asPublic

**Note:** A CRBasic command for declaring and dimensioning variables. Variables declared with Public can be monitored during datalogger operation.

orDim

**Note:** A CRBasic command for declaring and dimensioning variables. Variables declared with Dim remain hidden during datalogger operations.

. When declared as Public, structures can be viewed and manipulated remotely, similar to constants in aConstTable.

Special considerations apply when working with arrays of structures. When saving to a data table with repetitions (i.e., to a variable array), theFieldNamesinstruction is required (see Program Example #1). The labels specified in FieldNames() will prefix the values saved to the data table. Without FieldNames(), the compiler will generate a duplicate field name error.

Additionally, not all elements of an array of structures will be accessible from the **Table Monitor** in the software. Rather, the full contents of an array of structures is only visible by collecting them as a table. Therefore, if manual access to all array elements is required for modifying values, it is better to declare them as separate Public structures of the same type, rather than as an array.

Structures can also be accessed programmatically using Structure.Structure-variable syntax, which is analogous to Tablename.Fieldname syntax. Structure variables may be initialized just like any other variable.

Structures may also be passed into subroutines, used within functions or with pointers (that is., pointers may point to structures, see [Pointer-to-Structure Example](../Info/pointervariable.md#ptr_struc_example)).

The EndStructureType statement marks the end of the StructureType definition.

## Parameters

# Name

The name for the StructureType. The name is limited to 20 characters and must start with an alphabetical character. Note that the StructureType name cannot be any of the predefined table names in the datalogger; that is, Public, Status, Settings, or DataTableInfo. Multiple structures may be declared as the same StructureType.
