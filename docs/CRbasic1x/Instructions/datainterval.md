# DataInterval (Data Storage Interval)

The DataInterval statement is used to set the time interval for tables with data storage based on the datalogger's real-time clock.

## Syntax

DataInterval(TintoInt,Interval,Units,Lapses)

In the following simple program, the DataInterval instruction is used to store battery and panel temperature data to the Test table once per minute.

```
Public PTemp, BattVolt
```

DataTable (Test,True,-1)
DataInterval(0,1,Min,10)
Sample (1,BattVolt,FP2)
Sample (1,PTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (BattVolt)
PanelTemp (PTemp,15000)
CallTable Test
NextScan
EndProg

```

## Remarks


The DataInterval statement is inserted into a DataTable declaration following the DataTable instruction to establish a fixed interval on which the DataTable is run. This fixed interval defined by TintoInt (time into interval) and Interval is independent of, but must coincide with, the program execution interval. That is, the data interval must be integrally divisible by the program scan interval. The only exception to this rule is when DataInterval is used in a table that is the destination of a [GetDataRecord](getdatarecord.md) instruction.Beginning with OS2, the data interval for a GetDataRecord destination table is not required to be integrally divisible by the program scan interval. This results in correct timestamps in the case that a remote datalogger is storing data faster than the local program with GetDataRecord() maxrecords > 1.

When DataInterval is included in a data table, the datalogger will use only values from within the defined interval for time series processing. If an output interval is skipped (for example, the table was not called for any reason) processing memory that holds the intermediate values is reset the next time the data table is called. Thus, data from the previous interval is discarded.[OpenInterval](openinterval.md) can be included in the data table declaration to keep the data from the old interval in memory, and use it in calculating the time series data on the next interval. Note, however, that this means the data used for time series processing could span multiple output intervals.

Note that even if the DataInterval to store data to the DataTable is met, the Trigger Variable defined in the DataTable instruction must also be true or data storage will not occur.

The DataInterval instruction is optional. If it is not used, data is stored each time the CallTable is executed and a time stamp is stored with each record. Using a DataInterval is an efficient means of data storage, because a time stamp is not stored with each record. The datalogger still stores time but on a fixed interval (about once per 1K of memory used for the table). As each new record is stored, time is checked to ensure that the interval is correct. The time stamp for the data is calculated when data is retrieved from the datalogger, based on the number of records in the table and the last successful data storage event.

If the Interval parameter is set to 0, the data storage interval is the scan interval of the scan (main scan or slow sequence) from which the table is called. If a scan contains multiple calls to a table (for instance, within a For/Next loop) only one record is stored since the next scan interval has not occurred.

**NOTE:** DataInterval is typically not used for tables that are called conditionally (and in some instances may result in statistical data loss if included). The data storage efficiencies that would normally be gained are lost due to timestamp tracking markers. In this instance, it is more efficient for the datalogger to store a timestamp with each record.

Be careful to ensure that the interval defined by TintoInt and Interval occurs when a program scan occurs or no data will be stored.


## Parameters



# TintoInt (Time into Interval)


Allows an offset to the specified Interval. The Units for time are the same as for the Interval. This parameter must be an integer. Use of non-integers may result in the interval not evaluating as True when expected. In the DataInterval instruction, this parameter must be a constant (a variable is not allowed). If a variable is used in this parameter, it is recommended to define it as a Long.

TintoInt parameter example usage: if the Interval is set at 60 minutes, and TintoInt is set to 5, then data storage will occur at 5 minutes into the hour, every hour, based on the datalogger's real-time clock. If the TintoInt is set to 0, data storage will occur at the top of the hour.

Type: Constant (required for DataInterval instruction) or Variable (declared as Long, recommended).


# Interval (Time Interval)


The Interval is how frequently data will be stored to the DataTable, based on the datalogger's real-time clock. Enter the time interval on which the data in the table is to be recorded. The interval may be in milliseconds, seconds, or minutes, whichever is selected with the Units parameter. Enter 0 to make the data interval the same as the scan interval. DataInterval does not override the trigger in the DataTable instruction. If the trigger is not always true, it is a condition that must be met in addition to the time interval prior to data being stored.

Type: Constant (or expression that evaluates as a constant)


# Units


The units for the interval parameter. Right-click the parameter to display a list.

| Code | Description |
| --- | --- |
| msec | milliseconds |
| sec | seconds |
| min | minutes |
| hr | hours |
| day | days |
| mon | month (seconds into the month) |

Type: Constant

For the DataInterval instruction, the Units argument is used to specify the units on which the TintoInt and Interval arguments are based. When month (mon) is used for the Units parameter, the Interval parameter is specified in months and the TimeIntoInt parameter is specified as seconds into the month. The Interval is synchronized to the beginning of the year (January). If the current month of the year modulo the Interval equals 1 and the seconds into the current month matches the TimeIntoInt, then the condition is true. If TimeIntoInt is negative, then the True condition is in the current month.


# Lapses


A Lapse is any discontinuity of records in a DataTable. As each record is stored in a DataTable, the time is compared to the last record stored to ensure that a data storage interval has not been missed (that is, that a Lapse has not occurred). If a Lapse has occurred, a time stamp is stored along with the data. The Lapse argument allocates the additional memory in the data table for tracking these lapses and storing the timestamp (for more information, see [Lapses](../parameters/lapses1.md)). Entering a zero will cause every record in the DataTable to be time stamped, which requires an additional 16 bytes per record. If a negative value is entered, the datalogger will not adjust the timestamps of records due to lapses. Note that if several data storage intervals are missed in sequence, only one lapse is recorded for the missed records.
```
