# DataTable/EndTable (Define Output Table)

The DataTable instruction is used to define the name, trigger condition, and size of an output table. The EndTable statement designates the end of the output table.

## Syntax

```
DataTable
```

(Name,TrigVar,Size)

output trigger modifier

online storage destinations

output processing instructions

EndTable

Following is simple program measuring battery voltage and panel temperature. The DataTable/EndTable instructions are used to set up a table named Test.

```
Public PTemp, BattVolt

DataTable(Test,True,-1)
DataInterval (0,1,Min,10)
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

The DataTable declaration must be at the beginning of the code prior to the BeginProg instruction (the exception is when the DataTable is called from a [SlowSequence](slowsequence.md) scan;. in that instance, it can also be placed immediately following the SlowSequence instruction). A DataTable declaration begins with the DataTable statement and ends with the EndTable statement. Within the declaration are optional output trigger modifiers (DataInterval, DataEventor WorstCase), the online storage devices to which the data should be sent (CardOut), and the output processing instructions. .

It is a more efficient use of memory to include the DataInterval instruction when storing data on a fixed interval. If DataInterval is not included in the table declaration, a time stamp is stored in datalogger memory with every record. When DataInterval is in the table declaration, the datalogger still stores time but on a fixed interval (about once per 1K of memory used for the table). As each new record is stored, time is checked to ensure that the interval is correct. The time stamp for the data is calculated when data is retrieved from the datalogger, based on the number of records in the table and the last successful data storage event.

The Name parameter is the name given to the data table in the datalogger. Note that TableName cannot be any of the predefined table names in the datalogger; for example, Public, Status, Settings, or DataTableInfo. The TrigVar determines whether or not the data table is written to (0 = False, any non-zero number = true).

A DataTable is called in the program by using the [CallTable](calltable.md) instruction.

**NOTE:** Conditionally called data tables generally hold fewer records than time-interval driven data tables. Therefore, to avoid wasting data-storage memory, best practice is to use a fixed number of records for the size parameter of conditional tables, rather than using auto-allocate. If auto-allocate is used for the size of a conditional table, memory allocation will assume that the condition is always met, and a larger portion of memory will be allocated for the conditional table than is necessary.

**NOTE:** For information on retrieving data stored in data tables to use in your program, see [Data Table Access](../Info/datatableaccess.md).

## Parameters

# Name

The name for the output table. The table name is limited to 20 characters and must start with an alphabetical character. Note that TableName cannot be any of the predefined table names in the datalogger; for example, Public, Status, Settings, or DataTableInfo.

Type: Name

# TrigVar (Trigger Variable)

The constant, variable, or expression that will be tested to determine whether or not to trigger output to the DataTable. If the TrigVar is 0, a new record will not be saved to the datatable. If it is any non-zero number, a new record will be written.

| Logic | Value | Result         |
| ----- | ----- | -------------- |
| False | 0     | Do not trigger |
| True  | ≠ 0   | Trigger        |

Type: Constant, Variable, or Expression

# Size

Defines the number of records (or rows) that should be allocated for the DataTable. The number of values (or columns) in the DataTable is determined by the output processing instructions contained in the DataTable declaration. Size can be defined as a fixed number of records or as auto-allocate. To set the table size to a fixed number of records, enter that value. To set the size to autoallocate, enter a -1. If a table is set to auto-allocate, all memory that remains after creating fixed-sized tables will be allocated to this table. If multiple DataTables are declared with a -1 for size, the available memory will be divided among the tables. The datalogger attempts to allocate memory to the tables so that all tables are filled at the same time. By default, data storage memory sectors are organized as ring memory. When the ring is full, oldest data are overwritten by newest data. Using the [FillStop](fillstop.md) statement sets a program to stop writing to the data table when it is full, and no more data are stored until the table is reset.

For the CardOut instruction, enter **-1000** to set the size of the data table on the card to the size of the data table in datalogger memory.

**NOTE:** For extended-memory dataloggers, auto-allocated data tables are automatically written to the extended internal memory (which is 72 MB), unless [CardOut()](cardout.md) or [Tablefile()](tablefile.md) is used. In the case of CardOut() or Tablefile(), data from the CPU is streamed to the card in 1 KB frames and the internal extended memory is not used. Therefore, on extended-memory dataloggers, table fill times for auto-allocated tables on the CPU are greater if CardOut() or Tablefile() is not used. However, note that total final data storage for the table is greatly extended with external memory (up to 2 GB per table). In order to tell if the datalogger has extended internal memory, view the datalogger CPU Bytes Free in File Control. Dataloggers with extended internal memory show 30 MB Bytes Free for an empty CPU, compared to 1 MB Bytes Free for dataloggers that do not have extended internal memory.

Type: Constant (or expression that evaluates as a constant)

[For more information, seeAbout Data Tables.](../Info/datatables.md)
