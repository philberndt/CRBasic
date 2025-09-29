# SendGetVariables (Send or Retrieve Data from Host Datalogger)

The SendGetVariables instruction is used in a destination datalogger to send an array of values to the host datalogger, and/or retrieve an array of data from the host datalogger.

## Syntax

SendGetVariables(ResultCode,ComPort,NeighborAddr,PakBusAddr,Security,TimeOut,SendVariable,SendSwath,GetVariable,GetSwath)

In this example, a datalogger with PakBus address 109 is programmed to send three variables to a datalogger with an address of 1 using the TimeUntilTransmit and SendGetVariables instructions. Three variables are sent to address 1 when the TimeUntilTransmit = 0.

Note that the scan rate in this program must be 1 second. For the program that is run in the host datalogger (address 1), see the [Network](network.md) example.

```
'Declare Public Variables
Public RefTemp, batt_volt, RxData
Public TxData(3)
Public RxResponse
Public counter, time

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,RefTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)
counter=counter+1
If counter=100 Then
counter=0
EndIf
PanelTemp (RefTemp,15000)
Battery (Batt_volt)
TxData(1)=RefTemp
TxData(2)=Batt_volt
TxData(3)=counter

time = TimeUntilTransmit

If time = 0 Then
SendGetVariables(RxResponse,ComRS232,0,1,0000,0,TxData,3,RxData,1)
EndIf

CallTable Test
NextScan
EndProg
```

## Remarks

When the SendGetVariables instruction is used in a datalogger, data transmission times are controlled by a host datalogger. Most often, this instruction is preceded by the [TimeUntilTransmit](timeuntiltransmit.md) instruction to trigger the execution of the SendGetVariables instruction. The program in the host datalogger must contain the [Network](network.md) instruction, which sets the times that the destination dataloggers should respond. One of the values sent by the host datalogger is its clock value; the destination datalogger synchronizes its clock with that value.

If security is enabled in the host datalogger, it must be unlocked to level 2 for this instruction to be successful.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using this instruction. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

## Parameters

# ResultCode (Result Code)

The variable in which a response code for the transmission will be stored. A zero indicates a successful transaction. A positive value indicates that there was no response to the request from the remote. A negative value indicates some other type of error occurred. The codes that can be returned are:

| Code    | Description                                                                                                                                                                                   |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0       | Successful.                                                                                                                                                                                   |
| -1      | Response received but permission denied.                                                                                                                                                      |
| -2      | The Master datalogger cannot find this destination device in its Network instructions list.                                                                                                   |
| -16     | Table name and/or field name not present in the source datalogger, or the field is read only in the destination datalogger.                                                                   |
| -18     | Array in the source datalogger is not dimensioned large enough to accommodate the values to be sent or array in the destination datalogger is not large enough to accommodate values received |
| -20     | Out of Comms memory.                                                                                                                                                                          |
| -21     | Failed to route packet when routing is set to auto-discover and route is not yet known.                                                                                                       |
| -22     | Communication port buffer exceeded.                                                                                                                                                           |
| -27     | DialSequence/EndDialSequence returned False so communication did not occur.                                                                                                                   |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1.                         |

Type: Variable

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                           |
| ------------ | ------------------------------------- |
| ComRS232     | RS232 port of the datalogger          |
| ComME        | Datalogger CS I/O port; modem enabled |
| Com310       | Datalogger CS I/O port; COM310 modem  |
| Com320       | Datalogger CS I/O port, COM320 modem  |
| ComSDC7      | Datalogger CS I/O port; SDC7          |
| ComSDC8      | Datalogger CS I/O port; SDC8          |
| ComSDC10     | Datalogger CS I/O port; SDC10         |
| ComSDC11     | Datalogger CS I/O port; SDC11         |
| ComC1        | Datalogger control terminals 1 & 2    |
| ComC3        | Datalogger control terminals 3 & 4    |
| ComC5        | Datalogger control terminals 5 & 6    |
| ComC7        | Datalogger control terminals 7 & 8    |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

If a negative value is entered for the ComPort, the datalogger will not wait on a response from the destination device before proceeding to the next instruction. If autodiscovery is enabled (NeighborAddr = non-valid PakBus ID), this parameter is ignored.

When communicating over TCP/IP, use the variable returned by the TCPOpen function for the ComPort. If PakBusTCPServer setting is being used rather than TCPOpen, use the [Route](route.md)(-PakBusAddr ) instruction for the ComPort parameter.

# NeighborAddr (PakBus Address to Neighbor Datalogger)

Used to specify a static route to the destination datalogger (for example, thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of a "neighbor" datalogger that the host can go through to communicate with the destination datalogger).Note that the datalogger will attempt to use this route only until it "learns" a dynamic route to the destination datalogger.

If 0 is entered, the destination device is assumed to be a neighbor (i.e., the host datalogger can communicate with the destination directly). If a non-valid PakBus address is entered (a negative number or a number greater than 4094), the route to the destination device will be "autodiscovered" by other means in the PakBus network (such as beaconing or a Hello messages). If the instruction has a ResultCode parameter, an error code is returned until the route is discovered. When autodiscovery is used, the COMPort parameter is ignored.

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089.

Note that if the datalogger is configured as aleaf node

**Note:** A PakBus node at the end of a branch. When in this mode, the datalogger is not able to forward packets from one of its communication ports to another. It will not maintain a list of neighbors, but it still communicates with other PakBus dataloggers and wireless sensors. It cannot be used as a means of reaching (routing to) other dataloggers.

(as opposed as arouter

**Note:** A device configured as a router is able to forward PakBus packets from one port to another. To perform its routing duties, a datalogger configured as a router maintains its own list of neighbors and sends this list to other routers in the PakBus network. It also obtains and receives neighbor lists from other routers. Routers maintain a routing table, which is a list of known nodes and routes. A router will only accept and forward packets that are destined for known devices. Routers pass their lists of known neighbors to other routers to build the network routing system.

), setting the NeighborAddr to autodiscover should be used with care. A leaf node is aware only of its direct neighbors but has no knowledge of the rest of the PakBus network; therefore, it is not capable of "autodiscovering" a route to a destination datalogger that is not a direct neighbor. If the direct neighbors it can communicate with are not routers themselves, any communication packets sent will fail. Communication packets from the leaf node to the rest of the network also will fail if its direct router is no longer in the network.

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089, Konect GDS a value in the range of 4000 4050. 4095 is a broadcast address that can be used in a limited number of instructions.

# Security

The security code of the remote datalogger. 0 is entered for this parameter if no security is set in the destination datalogger.

Type: Integer

If security is enabled, it must be unlocked to level 3.

**NOTE:** If other data logger security settings, such as TCP password and PakBus Encryption are set, these must also match between remote and local data loggers for successful data logger to data logger communications to occur.

# TimeOut (Wait Time)

The amount of time, in 0.01 seconds, that the datalogger should wait for a response before considering the instruction to have failed. The datalogger waits for the TimeOut period to expire before proceeding to the next instruction.

If 0 is entered for this parameter, then the datalogger will use a time based on its known route to the destination device.

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

# SendSwath (Values to Send)

Specifies the number of values that will be sent to the destination datalogger. If values are being sent to multiple destination devices, this is the number being sent to each destination device.

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

For the SendGetVariables instruction, the GetVariables parameter must be dimensioned to the size of the GetSwath parameter.

# GetSwath (Values to Receive)

Specifies the number of values that will be received from the destination datalogger. If values are being received from multiple destination devices, this is the number from each destination device.

Type: Constant
