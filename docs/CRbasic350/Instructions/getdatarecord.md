# GetDataRecord (Retrieve Most Recent Data Record)

The GetDataRecord instruction retrieves the most recent record from a data table in a remote datalogger and stores the record in the local datalogger (the optional parameter MaxRecords can be used to retrieve older records from the table).

## Syntax

GetDataRecord(ResultCode,ComPort,NeighborAddr,PakBusAddr,Security,TimeOut,Tries,TableNo,DestTableName,MaxRecords[optional] )

### **_Example 1_**

In the following example programs, a datalogger is retrieving a record from the Temp data table from a remote datalogger.

Local Datalogger Program,CR350

```
Public TCTemp, BattV, Result

DataTable (TempDat,True,10000)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
SerialOpen (COM1,9600,0,0,10000)
Scan (10,Sec,3,0)
Battery (BattV)
GetDataRecord(Result,COM1,0,3000,0000,0,2,1,TempDat,1)
NextScan
EndProg
```

Remote Datalogger Program, CR300 Series

```
Public RefTemp, TCTemp

DataTable (TempDat,True,10000)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
SerialOpen (Com1,9600,0,0,10000)
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mV34,1,TypeT,RefTemp,True,0,4000,1.0,0)

CallTable (TempDat)
NextScan
EndProg
```

### **_Example 2_**

Retrieving records from a CR200(X) Series datalogger.

Local Datalogger Program,CR350

```
'Declare a table that has the same number and type of data
'fields as the table in the remote CR200(X). In this example,
'the remote CR200(X) has two data fields that are IEEE4,
'four byte floating point, numbers - "BattV" and "Counter".
'We do not count the dummy variable that has been added
'the remote CR200(X) table.
'Also, we need to make sure this table either a) does
'not contain a DataInterval() or if it does, that the
'Lapses parameter of DataInterval() is set to zero(0).

Public PTemp, BattV, Result
Dim PlaceHolder(2)

DataTable (CR200_OneMin,1,300)
Sample (2,PlaceHolder,IEEE4)
FieldNames ("BattV,Counter")
EndTable

DataTable (Test,1,-1)
DataInterval (0,1,Min,10)
Minimum (1,BattV,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

BeginProg
Scan (10,Sec,0,0)
PanelTemp (PTemp,60)
Battery (BattV)
CallTable Test

If IfTime (0,5,Min) Then
'every 5 minutes, attempt to retrieve any previously uncollected
'data records from the CR200's first (#1) table. If we get
'behind due to communication failures, collect a maximum of 10 of
'the most recent records from that table.
GetDataRecord(Result,COMRS232,0,2,0000,0,2,&h8001,CR200_OneMin,10)
EndIf

NextScan
EndProg
```

Remote Datalogger Program, CR200(X) Series

```
'If we wish to have another datalogger, like aCR350,
'collect a table from the CR200(X) using the GetDataRecord()
'instruction, we need make the first field a dummy variable
'that is equal to 0. The CR200(X) only uses a 4-byte record
'timestamp containing the number of seconds that has elapsed
'since 1 Jan 1990. TheCR350is expecting an 8-byte timestamp
'that contains the number of seconds since 1 Jan 1990 and the
'number of nanoseconds into the current second.
'The dummy variable in the CR200(X) will fill the
'need For the additional 4-bytes of timestamp required by
'theCR350.
```

Public BattV, counter
Dim NanoSec

DataTable (OneMin,1,100)
DataInterval (0,1,min)
Sample (1,NanoSec)'our dummy variable
Sample (1,BattV)
Sample (1,counter)
EndTable

BeginProg
NanoSec = 0'set our dummy variable to zero
Scan (10,sec)
Battery (BattV)
counter = counter + 1
CallTable OneMin
NextScan
EndProg

```

### ***Example 3***


In the following example programs, a datalogger is retrieving a record from the Temp and five minute Temp data tables from a remote datalogger. The Temp data table is configured first in the program so it is data table #1. The five minute Temp data table is configured next in the program so it is data table #2.

Time stamp information as well as data are retrieved from the remote datalogger and stored in the associated table on the local datalogger. A DataInterval instruction is NOT used in the local datalogger for either table. The Temp table in the remote datalogger does not contain a DataInterval instruction so the tables match exactly in the remote and local dataloggers. The data table structure for the five minute data is different since the DataInterval instruction is not used in the local datalogger. This requires the option TableNo in the GetDataRecord instruction to be modified by adding 32768 to the table number in this case 32768 is added to 2.

Local Datalogger Program,CR350

```

Public RefTemp'CR300 reference temperature
Public TCTemp'Thermocouple temperature
Public BattV'Battery Voltage
Public ResultTD'Result code for table #1
Public ResultTD5Min'Result code for table #2

'Data table #1
DataTable (TempDat,True,10000)
Sample (1,TCTemp,FP2)
Sample (1,RefTemp,FP2)
EndTable

'Data table #2
DataTable (TempDat5Min,True,1000)
Average (1,TCTemp,FP2,False)
Average (1,RefTemp,FP2,False)
EndTable

BeginProg
SerialOpen (COM1,115200,0,0,10000)
Scan (10,Sec,3,0)
Battery (BattV)
GetDataRecord(ResultTD,COM1,0,8,0000,0,2,1,TempDat,1)
If TimeIntoInterval (10,300,Sec)
GetDataRecord(ResultTD5Min,COM1,0,8,0000,0,2,32770,TempDat5Min,1)
EndIf
NextScan
EndProg

```
Remote Datalogger Program, CR300 Series

```

Public RefTemp'CR300 reference temperature
Public TCTemp'Thermocouple temperature

'Data table #1
DataTable (TempDat,True,10000)
Sample (1,TCTemp,FP2)
Sample (1,RefTemp,FP2)
EndTable

'Data table #2
DataTable (TempDat5Min,True,1000)
DataInterval (0,5,Min,10)
Average (1,TCTemp,FP2,False)
Average (1,RefTemp,FP2,False)
EndTable

BeginProg
SerialOpen (Com1,115200,0,0,10000)
Scan (1,Sec,3,0)
PanelTemp (RefTemp,60)
TCDiff (TCTemp,1,mV34,1,TypeT,RefTemp,True,0,60,1.0,0)

CallTable (TempDat)
CallTable (TempDat5Min)
NextScan
EndProg

```

## Remarks


The table definitions for the local and remote dataloggers must be identical (this includes field order, table size, and the declaration of units), unless 32768 (&H8000) is added to the TableNo parameter. For example, by adding 32768, table 1 is specified as 32769, table 2 is 32770, etc. Adding 32768 to the TableNo parameter means that the number of fields, and the data types must be the same, but the table names, number of records in the tables, units, and field names do not. In most cases, it is recommended to add 32768 to the TableNo parameter to avoid a table definitions mismatch.

If the records retrieved from a remote are from an interval driven, 32768 (&H8000) must be added to the TableNo parameter.

Beginning with OS6,[DataInterval](datainterval.md) for a GetDataRecord destination table is not required to be integrally divisible by the program scan interval. This results in correct timestamps in the case that the remote datalogger is storing data faster than the local program with GetDataRecord() maxrecords > 1. For example, if an aggregator datalogger is running a main scan of one minute, and a remote datalogger is storing measurements every 10 seconds, then in order get all of the most recent records from the remote datalogger, the DataInterval of the GetDataRecord() destination table would be 10 seconds and the maxrecords parameter of GetDataRecord() would be 6.

When retrieving records from a CR200(X), DataInterval should be omitted or the Lapses parameter of DataInterval should be set to 0 so that the local datalogger is set up to store a timestamp with each record (since the CR200(X) stores a timestamp with each record).

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using this instruction. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

This instruction includes a call to the data table (CallTable) defined by the DestTableName parameter.

**NOTE:** CR10XPB, CR23XPB, and CR510PB dataloggers do not respond to a GetDataRecord request from another datalogger.

When getting a data record from a CR200(X), there will always be a mismatch in the table signatures. Thus, 32768 must be added to the TableNo parameter.

To match the data itself between a CR1000 and CR200, define all the table elements in the CR1000 as IEEE4 and add a "dummy" variable that equals to 0 in the CR200 as the first variable. This dummy variable is a placeholder for the last four bytes of the time stamp.

The timestamp stored with the retrieved records is the timestamp of the remote datalogger.


## Parameters



# ResultCode (Result Code)


The ResultCode is the variable in which a response code for the transmission will be stored. A zero indicates a successful transaction. A positive value indicates that there was no response to the request from the remote. A negative value indicates some other type of error occurred. The codes that can be returned are:

| Code | Description |
| --- | --- |
| 0 | Successful. |
| -1 | Response received but permission denied. |
| -7 | Table definition mismatch; signatures do not match. |
| -8 | No records in the remote datalogger, no new records in the remote datalogger, or record length in the remote table is smaller than that in the local table. |
| -9 | Record length in the remote table is larger than that in the local table. The error will also occur if one datalogger s table is interval driven, and the other datalogger s table is non-interval driven (i.e., event driven). |
| -10 | Local DataTable() not triggered for output. |
| -16 | Table name and/or field name not present in the remote datalogger, or the field is read only in the destination datalogger. |
| -17 | Data type not supported. |
| -18 | Array in the sending datalogger is not dimensioned large enough to accommodate the values to be sent or array in the receiving datalogger is not large enough to accommodate values received |
| -20 | Out of Comms memory. |
| -21 | Failed to route packet when routing is set to auto-discover and route is not yet known. |
| -22 | Communication port buffer exceeded. |
| -27 | DialSequence/EndDialSequence returned False so communication did not occur. |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1. |


# ComPort (Communications Port)


The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description |
| --- | --- |
| ComUSB | USB port of the datalogger |
| ComRS232 | RS232 port of the datalogger |
| Com1 | Datalogger control terminals 1 (TX) & 2 (RX) |
| Com2 | COM2 port of the datalogger |
| Com3 | COM3 port of the datalogger |
| ComRF | Integrated RF communication |

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

**Note:** The degree to which the result of a measurement, calculation, or specification conforms to the correct value or a standard.

(as opposed as arouter

**Note:** A device configured as a router is able to forward PakBus packets from one port to another. To perform its routing duties, a datalogger configured as a router maintains its own list of neighbors and sends this list to other routers in the PakBus network. It also obtains and receives neighbor lists from other routers. Routers maintain a routing table, which is a list of known nodes and routes. A router will only accept and forward packets that are destined for known devices. Routers pass their lists of known neighbors to other routers to build the network routing system.

), setting the NeighborAddr to autodiscover should be used with care. A leaf node is aware only of its direct neighbors but has no knowledge of the rest of the PakBus network; therefore, it is not capable of "autodiscovering" a route to a destination datalogger that is not a direct neighbor. If the direct neighbors it can communicate with are not routers themselves, any communication packets sent will fail. Communication packets from the leaf node to the rest of the network also will fail if its direct router is no longer in the network.

When a leaf node is set to autodiscover it will attempt to communicate any time it is aware of a neighbor that is a router. This will result in an incrementing result code if that neighbor router is not aware of the destination address. If the leaf node's neighbor is not a router or if the leaf node has no neighbors, communication will not be attempted and a -21 is returned.


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

**NOTE:** In RF400 communication, the timeout should be sufficiently long to avoid collisions (the default of 0 should accomplish this, or use at least 500 ms).


# Tries


The number of times the local datalogger should attempt to retrieve the record from the remote datalogger before timing out and executing the next instruction.

Type: Constant or Variable


# TableNo (Table Number)


Specifies the table in the remote datalogger from which a record will be retrieved (GetDataRecord) or accepted (AcceptDataRecords). This is a numeric value that represents the data tables in the order they are declared in the program (for example, the first data table declared in the program is 1, the second data table declared in the program is 2, etc.).

If 32768 (&H8000) is added to the TableNo parameter, the table definitions in both the local and remote dataloggers do not need to be identical. As an example, to accomplish this for table 1, you can enter 32769, or 1+32768, or 1+&H8000. Table 2 would be 32770, etc.

Type: Constant


# DestTableName (Destination Table Name)


The name of the data table in the local datalogger where the incoming record(s) should be stored. Right click the parameter for a list of tables that have been declared in the program.

Type: Table Name


## Optional Parameter



# MaxRecords (Maximum Records to Retrieve)


Optional parameter.

If MaxRecords is set to a value less than 0, GetDataRecord will collect up to MaxRecords of uncollected data, beginning with the oldest missing record. If MaxRecords is greater than 0, the instruction will collect up to MaxRecords of uncollected data starting with the most recent record (and then going back and filling in the gaps or "holes"). The default value is 1, which collects only the most recent record. 1 is assumed if the parameter is not used.

Type: Constant

By default, GetDataRecord retrieves only the most recent record stored in a data table. The MaxRecords parameter is used to retrieve older records that have not yet been retrieved.

**NOTE:** If the parameter is some value other than 1, two exchanges will take place. The first is to determine the current record number and the second is to retrieve the data.
```
