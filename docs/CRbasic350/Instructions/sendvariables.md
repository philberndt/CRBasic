# SendVariables (Send Variables to Datalogger)

The SendVariables instruction is used to send value(s) from a variable or variable array to a data table in a destination datalogger or other PakBus device.

## Syntax

SendVariables(ResultCode,ComPort,NeighborAddr,PakBusAddr,Security,TimeOut,"TableName","FieldName",Variable,Swath)

### Example #1

In the following program, the datalogger will attempt to send 8 values to a destination datalogger with a PakBus ID of 1 each time Flag1 is high (non zero). The data values to be sent are from the TCTemp array.

```
'Declare variables
Public RXResponse
Public TCTemp(3), RefTemp
Public Flag(1)

'Main program
BeginProg
Scan (1 ,sec,0,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,3,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

'Toggle flag to send variables
If Flag(1) Then
SendVariables(RXResponse,ComRS232,0,1,0,0,"Public","Temp()",TCTemp(),3)
EndIf
NextScan
EndProg
```

### Example #2

The following program is an example of datalogger callback via a direct connection to the computer. In the following program the datalogger will send a variable named Callback to LoggerNet (PakBus address 4094) if the variable TC temp is greater than 26. After LoggerNet receives the variable "Callback" it will begin collecting data from the datalogger and store it into a file based on the data collection settings in the Setup window. It ignores the remaining parameters in the SendVariables instruction. The Scratch variable and swath value act only as placeholders in the instruction to avoid compiler errors.

Note that for a callback attempt to work in LoggerNet, the root device (COM port) and datalogger must be enabled for Call-Back in LoggerNet's Setup window. Additionally, best results are seen when the setting PakBus Port Always Open is enabled.

```
Public RefTemp, TCTemp, SCratch, SendResult

DataTable (TempDat,True,-1)
DataInterval (0,5,Sec,10)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
SerialOpen (COMRS232,9600,0,0,10000)
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

If TCTemp >26 Then
SendVariables(SendResult,COMRS232,0,4094,0000,0,"Public","Callback",Scratch,1)
EndIf
CallTable (TempDat)
NextScan
EndProg
```

## Remarks

Values can only be sent to the destination datalogger's Public or Status table. The Variable and Swath parameters are used to determine what values are sent to the destination datalogger. The first value to be sent is defined with Variable, and the number of values is specified by Swath. The most recent value(s) stored in the table are sent.

The SendVariables instruction can be used to initiate a datalogger call-back attempt to a computer running LoggerNet. In this instance, the TableName should be set to Public and the FieldName to Callback (see the second example program). When the software receives the string "Callback" from the datalogger, it initiates a data collection from the datalogger.

If security is enabled in the destination datalogger, it must be unlocked to level 2 for this instruction to be successful.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using this instruction. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

## Parameters

# ResultCode (Response Code)

The variable in which a response code for the transmission will be stored. A zero indicates a successful transaction. A positive value indicates that there was no response to the request from the remote. A negative value indicates some other type of error occurred. The codes that can be returned are:

| Code    | Description                                                                                                                                                                                   |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0       | Successful.                                                                                                                                                                                   |
| -1      | Response received but permission denied.                                                                                                                                                      |
| -16     | Table name and/or field name not present in the source datalogger, or the field is read only in the destination datalogger.                                                                   |
| -17     | Data type not supported.                                                                                                                                                                      |
| -18     | Array in the source datalogger is not dimensioned large enough to accommodate the values to be sent or array in the destination datalogger is not large enough to accommodate values received |
| -20     | Out of Comms memory.                                                                                                                                                                          |
| -21     | Failed to route packet when routing is set to auto-discover and route is not yet known.                                                                                                       |
| -22     | Communication port buffer exceeded.                                                                                                                                                           |
| -27     | DialSequence/EndDialSequence returned False so communication did not occur.                                                                                                                   |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1.                         |

Type: Variable

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                                  |
| ------------ | -------------------------------------------- |
| ComUSB       | USB port of the datalogger                   |
| ComRS232     | RS232 port of the datalogger                 |
| Com1         | Datalogger control terminals 1 (TX) & 2 (RX) |
| Com2         | COM2 port of the datalogger                  |
| Com3         | COM3 port of the datalogger                  |
| ComRF        | Integrated RF communication                  |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

If a negative value is entered for the ComPort, the datalogger will not wait on a response from the destination device before proceeding to the next instruction. If autodiscovery is enabled (NeighborAddr = non-valid PakBus ID), this parameter is ignored.

When communicating over TCP/IP, use the variable returned by the TCPOpen function for the ComPort. If PakBusTCPServer setting is being used rather than TCPOpen, use the [Route](route.md)(-PakBusAddr ) instruction for the ComPort parameter.

When sending values to a remote datalogger,[SerialOpen](serialopen.md) must be used in the remote datalogger s program to open the port for receiving the values.

# NeighborAddr (PakBus Address to Neighbor Datalogger)

Used to specify a static route to the destination datalogger (for example, thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of a "neighbor" datalogger that the host can go through to communicate with the destination datalogger).

If 0 is entered, the destination device is assumed to be a neighbor (i.e., the host datalogger can communicate with the destination directly). If a non-valid PakBus address is entered (a negative number or a number greater than 4094), the route to the destination device will be "autodiscovered" by other means in the PakBus network (such as beaconing or a Hello messages). If the instruction has a ResultCode parameter, an error code is returned until the route is discovered. When autodiscovery is used, the COMPort parameter is ignored.

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089.

Note that if the datalogger is configured as aleaf node

**Note:** A PakBus node at the end of a branch. When in this mode, the datalogger is not able to forward packets from one of its communication ports to another. It will not maintain a list of neighbors, but it still communicates with other PakBus dataloggers and wireless sensors. It cannot be used as a means of reaching (routing to) other dataloggers.

(as opposed to arouter

**Note:** A device configured as a router is able to forward PakBus packets from one port to another. To perform its routing duties, a datalogger configured as a router maintains its own list of neighbors and sends this list to other routers in the PakBus network. It also obtains and receives neighbor lists from other routers. Routers maintain a routing table, which is a list of known nodes and routes. A router will only accept and forward packets that are destined for known devices. Routers pass their lists of known neighbors to other routers to build the network routing system.

), setting the NeighborAddr to autodiscover should be used with care. A leaf node is aware only of its direct neighbors but has no knowledge of the rest of the PakBus network; therefore, it is not capable of "autodiscovering" a route to a destination datalogger that is not a direct neighbor. If the direct neighbors it can communicate with are not routers themselves, any communication packets sent will fail. Communication packets from the leaf node to the rest of the network also will fail if its direct router is no longer in the network.

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089. 4095 is a broadcast address that can be used in a limited number of instructions.

If the PakBusAddr parameter is negated, communication generated by this instruction will not be encrypted using PakBus Encryption.

# Security

The security code of the remote datalogger. 0 is entered for this parameter if no security is set in the destination datalogger.

Type: Integer

If security is enabled, it must be unlocked to level 3.

**NOTE:** If other data logger security settings, such as TCP password and PakBus Encryption are set, these must also match between remote and local data loggers for successful data logger to data logger communications to occur.

# TimeOut (Wait Time)

The amount of time, in 0.01 seconds, that the datalogger should wait for a response before considering the instruction to have failed. The datalogger waits for the TimeOut period to expire before proceeding to the next instruction.

If 0 is entered for this parameter, then the datalogger will use a time based on its known route to the destination device.

Type: Constant

**NOTE:** In RF400 communication, the timeout should be sufficiently long to avoid collisions (the default of 0 should accomplish this, or use at least 500 ms).

# "TableName" (Table Name)

The name of the DataTable which contains the values to retrieve (GetVariables) or the table to which values will be sent (SendVariables). TableName must be entered as a string (enclosed in quotes).

**NOTE:** Values can only be sent to or retrieved from an input location in an Edlog-programmed PakBus datalogger (CR10XPB, CR510PB, or CR23XPB). The TableName to be used is "Inlocs" (or "Public") and the FieldName is the input location label.

Type: String or Variable that evaluates as a String

For the SendVariables instruction, the TableName parameter is the data table in the destination PakBus device to which the value(s) will be sent. Values can be sent only to the Public (or Inlocs) or Status table.

# "Fieldname" (Field Name)

Used to specify the name of the variable(s) in the destination datalogger for (SendVariables) or retrieve from (GetVariables). If Swath is greater than 1, FieldName must be an array. FieldName must be entered as a string (enclosed in quotes). If the variable in the source datalogger has been assigned an Alias, the alias must be used for Fieldname unless the value requested is from the Public table. In this case, either the original name or the Alias name can be used (the exception is CR200 series dataloggers; they require that the Alias be used). If the requested Fieldname is from an output table of a CRBasic datalogger, and the output is something other than a sample, the output type suffix must be added to the variable name (for example, Temp_Avg).[For more information, seeCRBasic Program Structure.](../Info/crbasicprogramstructure.md)

Type: String or Variable that evaluates as a string

For the SendVariables instruction, the FieldName parameter is used to specify the name of the variable or variable array in the destination PakBus device to which data will be sent. If the variable in the destination datalogger has been assigned an Alias, either the original name or the Alias name can be used.

# Variable

The Variable parameter is usually a variable or variable array that holds the values to be sent to the destination PakBus device. This variable must be dimensioned equal to or greater than the Swath of values that will be sent.

The variable parameter may also be used to send an expression or a string expression to a destination PakBus device. In this case, the value of the expression is enclosed in quotes. For example, if the variable parameter is set to "Hello", Hello will be sent to the destination device.

# Swath (Number of Variables Sent/Received)

The number of variables that will be retrieved from or sent to the datalogger.

Type: Constant (or expression that evaluates as a constant)

**NOTE:** Values can only be sent to an input location in an Edlog-programmed PakBus datalogger (CR10XPB, CR510PB, or CR23XPB). The TableName to be used is "Inlocs" (or "Public") and the FieldName is the input location label.

If RF400 radios are being used for communication and retries are enabled, a negative value should not be used on the COMPort, and at least 2 seconds should be used for the TimeOut parameter.
