# SDI12SensorSetup/SDI12SensorResponse (SDI-12 Sensor Setup/Sensor Response)

The SDI12SensorSetup instruction sets up the datalogger to act as an SDI-12 sensor. The SDI12Sensor Response instruction holds the source of the data to send to the SDI-12 recorder.

## Syntax

SDI12SensorSetup(Repetitions,SDIPort,SDIAddress,ResponseTime)

SDI12SensorResponse(SDI12Source)

In the following program, the datalogger is set up to send four ascending values when requested by an SDI12 recorder.

```
'Declare Public Variables
Public RefTemp, batt_volt

'Main Program
BeginProg
Scan (1,Sec,0,0)
PanelTemp (RefTemp,4000)
Battery (batt_volt)
NextScan

SlowSequence
Do
SDI12SensorSetup(4,C1,0,4)
Delay(1,3,sec)
Dim Sdi12Source(4)
Dim i,count
count = count+1
For i = 1 To 4
Sdi12Source(i) = count + i
Next
SDI12SensorResponse(SDI12Source)
Loop
EndProg
```

## Remarks

The commands supported when the datalogger acts as an SDI12 Sensor are M, C, R, V, I, and ?.H commands (high-volume binary and ASCII data) are supported beginning with OS8.All commands support CRC support. Click [here](../Info/sdi12sensorsetupsupportedcommands.md) for additional information on the commands.

In most cases the SDI12SensorSetup/SDI12SensorResponse instructions would be placed in a [Do... Loop()](doloop.md) in a [SlowSequence()](slowsequence.md) scan to allow other functions to take place in the main program. (Scan/NextScan is not required in this SlowSequence for the sensor functionality to work.) When the SDI12SensorSetup instruction executes it stops execution of the sequence it is in and waits for a Break sequence from the recorder. If a valid address and command are received, then the preamble response is sent to the recorder and SDI12SensorSetup exits so instructions following it can execute. Execution continues as normal until the SDI12SensorResponse instruction executes. It is up to the user to make sure that the time between Setup and Response is not greater than the time reported back to the recorder as the response time. If this time is exceeded the sensor will not operate properly. Response time must be at least 1 second (the datalogger does not support 0 response time.)

**NOTE:** SDI12SensorSetup/SDI12SensorResponse can be placed within a Scan/NextScan in a SlowSequence. However, (1) you will have up to scan rate amount of delay before the datalogger can be polled after boot up, and (2) you will get skipped scans unless you are polling at the same rate as the scan rate. Therefore, using a Do...Loop is recommended.

**NOTE:** A datalogger can be assigned only one SDI12 address per SDI12 port (i.e., a single port cannot support multiple addresses).

## Parameters

# Reps (Repetitions)

The number of measurements to tell the SDI12 recorder to retrieve from the SDI12Sensor.

Type: Constant integer (or expression that evaluates as a constant).

# SDIPort (SDI Port)

The port to which the SDI-12 sensor is connected. Right-click the parameter to display a list. A numeric value is entered.

| Code | Description    |
| ---- | -------------- |
| C1   | Control Port 1 |
| C2   | Control Port 2 |

Type: Constant

# SDIAddress (SDI Address)

Enter the SDI12 address that will be affected by this instruction. (For SDI12SensorSetup instruction it is the address to assign to the datalogger. For the SDI12Recorder instruction it is the address of the SDI12 sensor.) Valid range are 0 through 9, A through Z, and a through z. Alphabetical characters should be enclosed in quotes (for example, "A"). The same commands can be sent to multiple sensors by specifying multiple addresses, enclosed in quotes, in this parameter (for example, 012 send the commands to sensors addressed as 0, 1, and 2). The Dest variable should include an extra dimension to accommodate multiple sensors, or, if Dest is declared as a string, all values from each sensor are returned into a separate element of the array (for example, Dest(1) contains all values from sensor 1, Dest(2) contains all values from sensor 2, etc.). .

When sending commands to multiple sensors, all of the C! commands are performed first, and then, while waiting on the C! commands to finish, non-C! commands are performed.

Type: Constant or Variable

# ResponseTime (Response Time)

The time, in seconds, that the SDI-12 recorder should wait before requesting data. This setting must be at least 1 second (the datalogger does not support 0 response time.)

Type: Constant
