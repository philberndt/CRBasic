# SendData (Send Data to a PakBus Device)

The SendData instruction is used to send a record from a data table to a destination PakBus device.

## Syntax

SendData(ComPort,NeighborAddr,PakBusAddr,DataTable,TableOption[optional] )

The following example program uses the SendData instruction to send the most recent record from Table1 to PakBus ID 4094 once per minute. If the PakBus ID 4094 device is the LoggerNet server, the data is stored in a data file on the PC.

```
Public Temp

DataTable (Table1,True,-1)
DataInterval (0,1,Sec,10)
Sample (1,Temp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (Temp,4000)
IF IfTime (0,1,Min) Then
SendData(ComRS232,0,4094,Table1)
EndIF
CallTable (Table1)
NextScan
EndProg
```

## Remarks

The SendData instruction sends the most recent record written to the data table. A record is sent only if new data has been stored to the table since the last record sent. This instruction can be used to send data to a PC running the LoggerNet server. When received, LoggerNet will store the data in a file under a name that follows its naming convention, as specified in the Setup window of the software.

SendData is a one-way transaction; thus, no error checking is performed to ensure the record was received in the destination PakBus device. SendData can be a more efficient means of data collection in some networks (for example, time division polling via RF). However, because only the most recent record is sent and because there is no error checking, there may be records that do not get sent to (or received by) the destination PakBus address. LoggerNet can be configured to collect these holes in the data if One Way Data Hole Collection is enabled in the Setup window and if scheduled data collection is not paused. Note, however, that particularly in a time division polled network, where SendData transactions have priority over the collection of holes in the data, hole collection may take a significant amount of time to complete. When sending one-way data to LoggerNet, it is recommended that Pakbus Port Always Open be enabled and a delay hang-up be added to the datalogger in the network map.

To SendData to another PakBus datalogger, the receiving datalogger's program must contain the [AcceptDataRecords](acceptdatarecords.md) instruction.

To SendData from a hidden table (TableHide instruction), append .secured to the data table name (for example, Mytable.secured).

If the sending datalogger is a GRANITE 9 or GRANITE 10, to send data to another datalogger you must append .ToDatalogger to the data table name (for example, Mytable.todatalogger). For other dataloggers (CR6, GRANITE6, CR1000X), appending .todatalogger to the table name is recommended but not required. Do not append .todatalogger if sending data to LoggerNet.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using this instruction. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

## Parameters

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

When communicating over TCP/IP, use the variable returned by the [TCPOpen](tcpopen.md) function for the ComPort. If PakBusTCPServer setting is being used rather than TCPOpen, use the [Route](route.md)(-PakBusAddr ) instruction for the ComPort parameter.

If autodiscovery is enabled (NeighborAddr = non-valid PakBus ID), this parameter is ignored.

# NeighborAddr (PakBus Address to Neighbor Datalogger)

Used to specify a static route to the destination datalogger (for example, thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of a "neighbor" datalogger that the host can go through to communicate with the destination datalogger).

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

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089. 4095 is a broadcast address that can be used in a limited number of instructions.

# DataTable (Data Table)

The name of the DataTable from which the last record should be sent. To send data from a hidden table ([TableHide](tablehide.md) instruction), append`.secured`to the data table name (for example,`Mytable.secured)`.

Type: Name

# TableOption (Table Option)

Used to specify which records should be sent to the destination PakBus device. This is an optional parameter. Right click the parameter to display a list of valid options:

| Code | Description                                                                                                                                                                                                                                                                                    |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Send the last record stored since the last execution of the instruction. This is the default option if the option code parameter is omitted.                                                                                                                                                   |
| -1   | Send all new records since the last execution of the instruction.                                                                                                                                                                                                                              |
| X    | Send the last X number of records (where X is an integer). If the number of available records is less than X, then all records will be sent. Note that this option can result in sending duplicate records, since it sends a discrete number of records with each execution of the instruction |

Sending multiple records (options -1 or X) increases the time it takes to execute the instruction (and thus, the total time for the datalogger to complete a scan). If skipped scans occur, increase the scan rate of the program or move the SendData to a [SlowSequence](slowsequence.md) scan.

Type: Constant or Variable
