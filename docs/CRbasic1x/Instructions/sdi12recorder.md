# SDI12Recorder (SDI-12 Recorder)

The SDI12Recorder instruction is used to retrieve the results from an SDI-12 sensor.

## Syntax

```
SDI12Recorder
```

(Dest,SDIPort,SDIAddress,"SDICommand",Multiplier, Offset,FillNAN(optional),WaitonTimeout(optional) )

### Example #1

This program uses the "C" SDI-12 command. The "C" command is the concurrent command and allows other measurements to occur while a datalogger is awaiting the sensor's response. Not all sensors support the use of the "C" command. Refer to your sensor manual to see which SDI-12 commands are supported.

```
'CR1000X Series

'Declare Variables and Units
Public BattV
Public PTemp_C
Public AirTemp
Public SR50A(2)
Public TCDT
Public SnowDepth

Alias SR50A(1)=DT
Alias SR50A(2)=Q

Units BattV=Volts
Units PTemp_C=Deg C
Units AirTemp=Deg C

'Define Data Tables
DataTable(Hourly,True,-1)
DataInterval(0,60,Min,10)
Average(1,SnowDepth,FP2,False)
Sample(1,AirTemp,FP2)
EndTable

DataTable(Daily,True,-1)
DataInterval(0,1440,Min,10)
Minimum(1,BattV,FP2,False,False)
EndTable

'Main Program
BeginProg
'Main Scan
Scan(1,Sec,1,0)
'Default Datalogger Battery Voltage measurement 'BattV'
Battery(BattV)
'Default Temperature measurement 'PTemp_C'
PanelTemp (PTemp_C,15000)
'107 Temperature Probe measurement 'AirTemp'
Therm107(AirTemp,1,1,Vx1,0,15000,1,0)

'SR50A Sonic Ranging Sensor (SDI-12 Output) measurements 'DT', 'Q', 'TCDT', and 'SnowDepth'
SDI12Recorder(SR50A(),C1,"0","C1!",1,0)
TCDT=DT*SQR((AirTemp+273.15)/273.15)
SnowDepth=2.5-TCDT

'Call Data Tables and Store Data
CallTable Hourly
CallTable Daily
NextScan
EndProg
```

### Example #2

This program runs SDI-12 measurements at a slower rate by placing it in a slow sequence. A scan in a slow sequence behaves like the main scan, except it pauses whenever the datalogger is busy with the main scan. The SDI-12 measurement process takes several steps, and these steps are broken up and scheduled to take place between main scans.

```
'CR1000X Series

'Declare Variables and Units
Public BattV
Public PTemp_C
Public AirTemp
Public SR50A(2)
Public TCDT
Public SnowDepth

Alias SR50A(1)=DT
Alias SR50A(2)=Q

Units BattV=Volts
Units PTemp_C=Deg C
Units AirTemp=Deg C

'Define Data Tables
DataTable(Hourly,True,-1)
DataInterval(0,60,Min,10)
Average(1,SnowDepth,FP2,False)
Sample(1,AirTemp,FP2)
EndTable

DataTable(Daily,True,-1)
DataInterval(0,1440,Min,10)
Minimum(1,BattV,FP2,False,False)
EndTable

'Main Program
BeginProg
'Main Scan
Scan(1,Sec,1,0)
'Default Datalogger Battery Voltage measurement 'BattV'
Battery(BattV)
'Default Temperature measurement 'PTemp_C'
PanelTemp (PTemp_C,15000)
'107 Temperature Probe measurement 'AirTemp'
Therm107(AirTemp,1,1,Vx1,0,15000,1,0)

'Call Data Tables and Store Data
CallTable Hourly
CallTable Daily
NextScan

SlowSequence

Scan (5,Sec,3,0)
'SR50A Sonic Ranging Sensor (SDI-12 Output) measurements 'DT', 'Q', 'TCDT', and 'SnowDepth'
SDI12Recorder(SR50A(),C1,"0","M1!",1,0)
TCDT=DT*SQR((AirTemp+273.15)/273.15)
SnowDepth=2.5-TCDT
NextScan
EndProg
```

### Example #3

This program uses three SDI12Recorder instructions to measure three SDI-12 sensors on the same control port. Because the sensors use the same control port,[SemaphoreGet and SempaphoreRelease](semaphoregetsemaphorerelease.md) are used to avoid a hardware conflict. Without SempaphoreGet and SemaphoreRelease,NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is likely to occur for one or more SDI-12 measurements due to resource conflicts with the control port.

```
'CR1000X Series

'Declare Variables
Public PTemp, Batt_volt
Public Sensor1(2)
Public Sensor2(2)
Public Sensor3(2)

BeginProg
Scan (5,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
NextScan

SlowSequence
Scan (10,Sec,100,0)
SemaphoreGet(1)
SDI12Recorder(Sensor1(),C1,1,"M!",1.0,0)
SemaphoreRelease(1)
NextScan
EndSequence

SlowSequence
Scan (30,Sec,100,0)
SemaphoreGet(1)
SDI12Recorder(Sensor2(),C1,2,"M!",1.0,0)
SDI12Recorder(Sensor3(),C1,3,"M!",1.0,0)
SemaphoreRelease(1)
NextScan
EndSequence
EndProg
```

## Remarks

The SDI12Recorder instruction sends the command specified by the SDI12Command parameter as (address)SDI12Command!. The M! and C! commands are used to set up the sensor for measurement and the D0! command is used to retrieve the measurement value. The difference in the M! and C! commands is that when the M! command is issued, the datalogger pauses its operation and waits until the sensor timeout expires or it receives the data from the sensor before continuing. When the C! command is issued, the datalogger continues with its program without pausing and queries the sensor for values on subsequent scans. If the sensor response time is set to 0, the datalogger will issue the C! or M! command, immediately followed by the D0! command. If the datalogger receives no response it will send the command a total of three times, with three retries with each attempt, or until a response is received. If no response is received,NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is recorded in the first element of the Dest array. The optional parameter, FillNAN, may be used to fill the entire or part of the Dest array with NAN (see the following FillNAN parameter help).

When using the C! command, if the sensor response time is greater than the scan time, the datalogger will issue the C! command and set a timer, which, when elapsed will trigger the datalogger to issue the D0! command on the next scan. If the sensor response time is less than the datalogger scan time, after the datalogger retrieves a measurement another C! command is issued. The data from this command is retrieved at the next datalogger scan interval. This algorithm ensures that data is retrieved from the sensor on each scan interval where new data is available.

For C! command functionality, after getting the initial response from the sensor including the time until measurements are ready and the number of measurements that will be sent, the SDI12Recorder instruction will finish executing, and the program will move on. On the next execution of an SDI12Recorder instruction after the time has passed for measurements to be ready, the data from the previously sent C! command will be retrieved with D! commands before sending out the next command. It is crucial in the case that you are using the C! command and not using the WaitonTimeout parameter, that the SDI12Recorder instruction that is executed to retrieve the data is the same instance of the instruction that sent out the C! command. If another instance of this instruction is executed and tries to retrieve the data for the C! command, undefined behavior can occur. Thus, giving an SDI12Recorder sending the C! command its own Scan (via a SlowSequence) is usually the best idea.

If you need to send different C! commands to the same address (for example, C1! And C9!), use variables for the Dest and SDICommand parameters and reassign them to the appropriate values before each call to SDI12Recorder to avoid undefined behavior. This way only one instance of the SDI12Recorder instruction to a given address is called in the Scan.

The M! command always causes the program to wait inside the SDI12Recorder instruction until enough time has passed that measurements are ready and the datalogger retrieves them. The optional WaitonTimeout parameter will cause an SDI12Recorder instruction sending a C! command to wait inside the instruction until the requested measurements are ready, retrieving the data with a D! command before moving on to the next instruction. This allows the SDI12Recorder instruction executing a C! command to block program execution as it would in the case of the M! command. The WaitonTimeout parameter adds the ability to work with multiple sensors in a single instruction call.

The SDICommand parameter can be a variable, thus, it can be conditionally set to C! or C (or M! or M). If a C (or M) command is used, a D command is issued to pick up only the data measured by a previous C! command but no new request for data is issued. This facilitates data retrieval from a previous request if it is pending. Note that the same instance of the SDI12Recorder must be used for this to work (not two separate instructions), since information on how many values to retrieve is held locally by the instruction.

If multiple sensors are measured on the same SDI-12 port, each sensor requires a unique SDI-12 address. In addition, when separate SDI12Recorder instructions occur in different but concurrent scans, use [SemaphoreGet and SemaphorRelease](semaphoregetsemaphorerelease.md) before and after SDI12Recorder to prevent a resource conflict that may result inNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

(see example # 3).

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the SDI12Recorder instruction, the Dest parameter must have enough elements to store all the data that is returned by the SDI-12 sensor or a 'variable out of range' error will result during the execution of the instruction. NAN is returned in the first element of the array if the sensor is busy with terminal commands, if the command sent is an invalid command, or if the sensor aborts with carriage return/line feed and there is no data.

# SDIPort (SDI Port)

The port to which the SDI-12 sensor is connected. Right-click the parameter to display a list. A numeric value is entered.

| Code | Description    |
| ---- | -------------- |
| C1   | Control Port 1 |
| C3   | Control Port 3 |
| C5   | Control Port 5 |
| C7   | Control Port 7 |

Type: Constant

# SDIAddress (SDI Address)

Enter the SDI12 address that will be affected by this instruction. (For SDI12SensorSetup instruction it is the address to assign to the datalogger. For the SDI12Recorder instruction it is the address of the SDI12 sensor.) Valid range are 0 through 9, A through Z, and a through z. Alphabetical characters should be enclosed in quotes (for example, "A"). The same commands can be sent to multiple sensors by specifying multiple addresses, enclosed in quotes, in this parameter (for example, 012 send the commands to sensors addressed as 0, 1, and 2). The Dest variable should include an extra dimension to accommodate multiple sensors, or, if Dest is declared as a string, all values from each sensor are returned into a separate element of the array (for example, Dest(1) contains all values from sensor 1, Dest(2) contains all values from sensor 2, etc.). .

When sending commands to multiple sensors, all of the C! commands are performed first, and then, while waiting on the C! commands to finish, non-C! commands are performed.

Type: Constant or Variable

# SDICommand (SDI Command)

Used to specify the command strings that will be sent to the sensor. The command must be enclosed in quotes.

| Command   | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| ?!        | Address query                                                            |
| aAb!      | Change address (where a = current address and b = new address)           |
| HB!       | High-volume binary data(supported with OS2and greater)                   |
| HA!       | High-volume ASCII data(supported with OS2and greater)                    |
| C!        | Initiate concurrent measurements                                         |
| CC!       | Initiate concurrent measurements (with checksum)                         |
| I!        | Send identification (destination variable must be formatted as a string) |
| M!        | Initiate measurements                                                    |
| MC!       | Initiate measurements (with checksum)                                    |
| M1! - M9! | Additional measurement commands specified by the SDI-12 sensor           |
| RC!       | Continuous measurement (with checksum)                                   |
| R0! - R9! | Continuous measurement commands                                          |
| V!        | Initiate verify sequence                                                 |
| X!        | Extended commands (destination variable must be formatted as a string)   |

If a check summed command fails, a NAN will be returned and the command will be retried.

Type: Constant or variable

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression

# FillNAN (Fill NAN)

An optional parameter that indicates how NAN values due to bad sensor readings are recorded. The default behavior if this parameter is not present is that NAN is written only to the first element in the array and the remaining elements of the array contain the last known good values. The options for this parameter are:

- **0 **: Default behavior; write NAN only to the first element in the array

- **-1 **: Fill the entire array with NAN

- **>0**: Fill the specified number of elements in the array with NAN, beginning with the starting element defined by Dest

Type: Constant

# WaitonTimeout (Wait for Timeout)

An optional parameter that when set to 1 specifies that the C! command will wait inside the instruction for the sensor s advertised timeout and then issue the D! command to record the data. This allows multiple sensors to be measured with a single instruction call.

Type: Constant
