# AcceptDataRecords

The AcceptDataRecords instruction is used to set up a datalogger to accept and store records from a remote datalogger.

## Syntax

AcceptDataRecords(PakBusAddr,TableNo,DestTableName)

Two example programs follow. The first is a program for the datalogger that will be accepting records from a remote datalogger. The second is a program for the remote datalogger. For testing, a two-lead cable was used to connect theCOM1ports of the two dataloggers.

### AcceptDataRecords Instruction

```
Public Batt_volt
Public Temp, Counter As Long

DataTable (CR350Table,True,1000)
Sample (1,Temp,FP2)
Sample (1,Counter,Long)
EndTable

BeginProg
SerialOpen (COM1,9600,0,0,10000)
Scan (1,Sec,3,0)
Battery (Batt_volt)
'set the datalogger up to accept & store records from address 3000
'32768 is added to 1 (table 1) for the table number parameter
'so table definitions do not have to match
AcceptDataRecords(3000,32769,CR350Table)
NextScan
EndProg
```

### SendData instruction

```
Public Temp, Counter As Long

DataTable (CR350Table,True,1000)
DataInterval (0,10,Sec,10)
Sample (1,Temp,FP2)
Sample (1,Counter,Long)
EndTable

BeginProg
SerialOpen (COM1,9600,0,0,10000)
Scan (1,Sec,3,0)
PanelTemp (Temp, 60)
Counter=Counter+1
'send data to address 1010
SendData(COM1,0,1010,CR350Table)
CallTable (CR350Table)
NextScan
EndProg
```

## Remarks

The AcceptDataRecords instruction is used in place of the [CallTable](calltable.md) instruction to store data sent by a remote datalogger. The remote datalogger sends the data using the [SendData](senddata.md) instruction.

The table definitions for the local and remote dataloggers must be identical, unless 32768 (&H8000) is added to the TableNo parameter. The local data table cannot be an interval driven table, though the remote data table can be interval driven. Thus, in most cases you will need to add the 32768. When it is added, the number of fields and the data types must be the same, but the table name, number of records in the table, and field names do not. It is a more efficient use of memory to set a fixed number of records in the data table that will hold the incoming data, rather than let the datalogger autoallocate table size. Without a data interval, memory is allocated for a table as if the table is executed with each scan.

The timestamp for the data stored in the local table (DestTableName) is the timestamp of the record in the remote datalogger.

Since this instruction calls the table when records are received from the remote, you should not include a call to the same table (CallTable) elsewhere in the program.

## Parameters

# PakBusAddr (PakBus Address)

The PakBus address of the remote datalogger from which records should be accepted. If 0 is entered, the datalogger will accept records from any PakBus address.

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089. 4095 is a broadcast address that can be used in a limited number of instructions. Typically, dataloggers are assigned addresses less than 4000.

# TableNo (Table Number)

Specifies the table in the remote datalogger from which a record will be retrieved (GetDataRecord) or accepted (AcceptDataRecords). This is a numeric value that represents the data tables in the order they are declared in the program (for example, the first data table declared in the program is 1, the second data table declared in the program is 2, etc.).

If 32768 (&H8000) is added to the TableNo parameter, the table definitions in both the local and remote dataloggers do not need to be identical. As an example, to accomplish this for table 1, you can enter 32769, or 1+32768, or 1+&H8000. Table 2 would be 32770, etc.

Type: Constant

# DestTableName (Destination Table Name)

The name of the data table in the local datalogger where the incoming record(s) should be stored. Right click the parameter for a list of tables that have been declared in the program.

Type: Table Name
