# SendFile (Send File to a PakBus Device)

The SendFile instruction is used to send a file from the datalogger to another PakBus datalogger.

## Syntax

SendFile(ResultCode,ComPort,NeighborAddr,PakBusAddr,Security,TimeOut,"LocalFile","RemoteFile")

The following example program sends a file named DatCR300.dat that is stored on the datalogger'sCPUdrive to another datalogger with a PakBus ID of 1086 when Flag(1) is high. A PC running LoggerNet (PakBus ID 4094) is used to route the packet from one datalogger to the other. Note that in this instance, the PakBus ports in LoggerNet must be bridged or the SendFile will fail.

```
Public RefTemp, TCTemp, Result, OutStat AS Boolean, LastFileName AS String * 30
Public Flag(1)

DataTable (TestTab,True,-1)
TableFile ("CPU:DatCR300",8,2,10,0,0,OutStat,LastFileName)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

If Flag(1) Then
SendFile(Result,ComRS232,4094,1086,0000,0,LastFileName,LastFileName)
EndIf
CallTable (TestTab)
NextScan
EndProg
```

## Remarks

The SendFile instruction can be used along with the TableFile instruction to send a file to a remote PakBus datalogger.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using this instruction. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

## Parameters

# Result

The variable in which a response code for the transmission will be stored. A zero indicates a successful transaction. A positive value indicates that there was no response to the request from the remote. A negative value indicates some other type of error occurred. The codes that can be returned are:

| Code    | Description                                                                                                                                                                                  |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0       | Successful.                                                                                                                                                                                  |
| -1      | Response received but permission denied.                                                                                                                                                     |
| -2      | Illegal drive, cannot open the file in the remote, or destination drive full.                                                                                                                |
| -9      | Wrong fragment sent to the remote.                                                                                                                                                           |
| -14     | Too many files opened in the remote.                                                                                                                                                         |
| -16     | Table name and/or field name not present in the remote datalogger, or the field is read only in the destination datalogger.                                                                  |
| -17     | Data type not supported.                                                                                                                                                                     |
| -18     | Array in the sending datalogger is not dimensioned large enough to accommodate the values to be sent or array in the receiving datalogger is not large enough to accommodate values received |
| -20     | Out of Comms memory.                                                                                                                                                                         |
| -21     | Failed to route packet when routing is set to auto-discover and route is not yet known.                                                                                                      |
| -22     | Communication port buffer exceeded.                                                                                                                                                          |
| -25     | Cannot seek in the file.                                                                                                                                                                     |
| -26     | Cannot open the local file.                                                                                                                                                                  |
| -27     | DialSequence/EndDialSequence returned False so communication did not occur.                                                                                                                  |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1.                        |

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                                  |
| ------------ | -------------------------------------------- |
| ComUSB       | USB port of the datalogger                   |
| ComRS232     | RS232 port of the datalogger                 |
| Com1         | Datalogger control terminals 1 (TX) & 2 (RX) |
| ComRF        | Integrated RF communication                  |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

If a negative value is entered for the ComPort, the datalogger will not wait on a response from the destination device before proceeding to the next instruction. If autodiscovery is enabled (NeighborAddr = non-valid PakBus ID), this parameter is ignored.

When communicating over TCP/IP, use the variable returned by the TCPOpen function for the ComPort. If PakBusTCPServer setting is being used rather than TCPOpen, use the [Route](route.md)(-PakBusAddr ) instruction for the ComPort parameter.

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

# Security

The security code of the remote datalogger. 0 is entered for this parameter if no security is set in the destination datalogger.

Type: Integer

If security is enabled, it must be unlocked to level 3.

**NOTE:** If other data logger security settings, such as TCP password and PakBus Encryption are set, these must also match between remote and local data loggers for successful data logger to data logger communications to occur.

# TimeOut (Wait Time)

The amount of time, in 0.01 seconds, that the datalogger should wait for a response before considering the instruction to have failed. The datalogger waits for the TimeOut period to expire before proceeding to the next instruction.

If 0 is entered for this parameter, then the datalogger will use a time based on its known route to the destination device.

Type: Constant

Note that in RF400 communication, the timeout should be sufficiently long to avoid collisions (the default of 0 should accomplish this, or use at least 500 ms).

# LocalFile (Local File)

The Device and Filename, enclosed in quotes, where the file to be sent is stored ("Device:FileName").The only device choice for this datalogger is CPU.

Type: String variable

# RemoteFile (Remote File)

The Device and Filename, enclosed in quotes, where the file should be stored on the remote datalogger ("Device:FileName"). PakBus datalogger device options are: CPU: = datalogger CPU drive; CRD: = memory card; USB: = SC115 or external USB drive; CS9: = SC115; or USR: = user defined drive.

**NOTE:** In GRANITE Datalogger Modules, the USB device is an external USB drive and the CS9 device is an SC115. In all other dataloggers, the USB device is the SC115.

Type: String variable
