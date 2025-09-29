# Network

The Network instruction is used in conjunction with the [SendGetVariables](sendgetvariables.md) instruction to configure destination dataloggers in a PakBus network to send data to (and receive data from) the host.

## Syntax

Network(ResultCode,Reps,BeginAddr,TimeIntoInterval,Interval,Gap,GetSwath,GetVariable,SendSwath,SendVariable)

This example program is written to set up two dataloggers (addresses 109 and 110) to send and receive data via a [SendGetVariables](sendgetvariables.md) instruction in each of the destination devices. The Network instruction sends setup information to the destination dataloggers, which includes variable names and time values that are used by the [TimeUntilTransmit](timeuntiltransmit.md) instruction.

For the program that is run in the destination devices, see the [SendGetVariables](sendgetvariables.md) example.

```
'Declare Public Variables
Public PTemp, batt_volt,Result(2),Rx(2,3),Tx(2,1)

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
'increment counter for value to send to destination devices
Tx(1,1)=Tx(1,1)+1
If Tx(1,1)=60 Then
Tx(1,1)=0
EndIf

'put counter into Tx dimensioned array to send to destination devices
Tx(2,1)=Tx(1,1)

'Set up dataloggers to response within 10 sec of the 30 sec interval
'2 second gap between responses
Network(Result(),2,109,10,30,2,3,Rx(),1,Tx())
CallTable Test
NextScan
EndProg
```

## Results

The Network instruction is used in the host datalogger; the [SendGetVariables](<JavaScript:TL_5193950.HHClick()>)instruction is used in the destination dataloggers. When the program is compiled, the Network instruction sets up the host datalogger to respond to the SendGetVariables messages that are initiated by the remote dataloggers, transmits the host datalogger's clock value, and assigns a time slot to all the destination devices during which they should respond with their data. The destination dataloggers synchronize their clocks with the host datalogger's time value. With each SendGetVariables execution, the remote dataloggers send/get data, request a clock value, and request the next time to transmit. The time slot assigned to each of the destination devices is based on the number of destination devices (Reps), the Interval/TimeIntoInterval, and the Gap, so that all destination devices respond by the next time the Interval/TimeIntoInterval occurs.

When the Network instruction is run during program execution (after compile time), it records in the ResultCode variable whether or not the remote dataloggers have reported in since the instruction was last executed. The ResultCode is set to -1 when a SendGetVariables is received. Thus, if the rate at which the Network instruction is executed is the same as the rate the remotes are reporting in; the ResultCode is 0 if communication is successful. If the Network instruction is executed once per second, and the remotes report in once per minute, then the ResultCode would increment up to 59 and then be set back to -1 when the remote responded (and incremented by 1 with the next Network execution).

The Network instruction can be placed within the scan or outside the scan (but after BeginProg). If placed outside the scan, the ResultCode variable is set to -1 when a remote responds, but otherwise, the variable is never incremented to track successful communication back from the remotes.

If the PakBus addresses of remote dataloggers are not in consecutive order, multiple Network instructions can be used in the program.

## Parameters

# ResultCode (Result Code)

The variable in which a response code for the transmission will be stored. Each time the Network instruction is executed, the ResultCode for each destination device is incremented by 1. When a response is received back from a destination device, the ResultCode is set to -1.

If the Reps parameter (the number of destination devices) is greater than one, then ResultCode should be dimensioned as an array large enough to accommodate a result from each of the destination devices.

Type: Variable or Variable Array

# Reps (Number of Destination Dataloggers)

The number of destination dataloggers with which the host datalogger will be communicating.

Type: Constant

# BeginAddr (Beginning Address)

The PakBus address of the first destination datalogger with which the host will be communicating. If the addresses for the devices to be communicated with using this instruction are in sequential order, BeginAddr can be a scalar variable containing the address of the first device. If the addresses are not in sequential order, BeginAddr is an array declared as type Long, where each element of the array contains the PakBus address of a device to be communicated with.

Type: Variable or variable array containing an Integer, 1 through 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089.

# TimeIntoInterval (Time into Interval)

The TimeIntoInterval parameter is used along with the Interval parameter to provide a time to the destination dataloggers by which they all must communicate. The TimeIntoInterval is essentially a maximum offset for the interval parameter (for example, if the interval is 60 seconds and the TimeIntoInterval is 15, communication should be completed by 15 seconds after the top of the minute, each minute). This value is entered in seconds.

Type: Constant

# Interval

Specifies how often the destination dataloggers should send data back to the host. This value is entered in seconds. It should be the same interval as the scan interval, or an even interval into the scan interval.

Type: Constant

# Gap (Delay between Communications)

Used to specify a delay, in seconds, between communication from each of the destination dataloggers. If 0 is entered, the default value, based on the host datalogger's routing table, will be used. If a negative number is entered, no delay will occur between the data transmission by each of the destination dataloggers.

Type: Constant

# GetSwath (Values to Receive)

Specifies the number of values that will be received from the destination datalogger. If values are being received from multiple destination devices, this is the number from each destination device.

Type: Constant

# GetVariables (Variables to Retrieve)

The variable or variable array in which values retrieved from the destination datalogger will be stored. If multiple values are being retrieved from multiple dataloggers, this parameter must be dimensioned to a multi-dimensional array large enough to accommodate the values returned. For example, if 2 values are being returned by each of 4 destination devices, the variable should be dimensioned to (4, 2).

| Destination device #, Value = Store In | Destination device #, Value = Store In |
| -------------------------------------- | -------------------------------------- |
| Destination device 1, value 1 = (1, 1) | Destination device 1, value 2 = (1, 2) |
| Destination device 2, value 1 = (2, 1) | Destination device 2, value 2 = (2, 2) |
| Destination device 3, value 1 = (3, 1) | Destination device 3, value 2 = (3, 2) |
| Destination device 4, value 1 = (4, 1) | Destination device 4, value 2 = (4, 2) |

Type: Variable or variable array

# SendSwath (Values to Send)

Specifies the number of values that will be sent to the destination datalogger. If values are being sent to multiple destination devices, this is the number being sent to each destination device.

Type: Constant

# SendVariable (Variables to Send)

The variable(s) that will be sent from this datalogger to the destination datalogger(s). If sending a Public variable that has been Aliased, either the Alias or the original variable name can be used. If multiple values are being sent to multiple destination dataloggers, this parameter must be a multi-dimensional array containing the values to be sent to each destination device.

For instance, if you are sending 2 values to each of 3 destination devices, the variable should be dimensioned to (3, 2). The results would be:

| Destination device #, Value = Send From | Destination device #, Value = Send From |
| --------------------------------------- | --------------------------------------- |
| Destination device 1, value 1 = (1, 1)  | Destination device 1, value 2 = (1, 2)  |
| Destination device 2, value 1 = (2, 1)  | Destination device 2, value 2 = (2, 2)  |
| Destination device 3, value 1 = (3, 1)  | Destination device 3, value 2 = (3, 2)  |

Type: Variable or variable array

**NOTE:** This instruction does not initiate communication with the destination dataloggers; communication is initiated by the destination devices using the SendGetVariables instruction.
