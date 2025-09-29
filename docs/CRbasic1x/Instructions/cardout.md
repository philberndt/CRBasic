# CardOut (Save New DataTable to a Memory Card)

The CardOut instruction is used to create a new DataTable that will be saved on a memory card (CRD drive).

Note, when storing high-frequency data, or when storing data to cards greater than 2 GB, the [TableFile](tablefile.md) instruction with`Option 64`may be a better method to write final storage data to a card.[For more information, seeMethods for Writing Data to a Card.](../Info/MethodsWriteDatatoCard.md)

## Syntax

```
CardOut
```

(StopRing,Size)

The following example shows the use of the CardOut instruction to store a DataTable to a memory card. 1000 records will be stored to the card; it is formatted as ring memory.

```
Public Batt_volt, RefTemp, TCTemp

DataTable (Table1,True,-1)
DataInterval (0,1,Min,10)
CardOut(0,1000)
Sample (1,Batt_volt,FP2)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
```

Battery(Batt_volt)
PanelTemp (RefTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

CallTable (Table1)
NextScan
EndProg

```

## Remarks


A CardOut instruction must be entered within each DataTable declaration for which data should be written to a memory card. The data table is saved to both the datalogger s internal memory and the card. The internal memory is used as a buffer to transfer data to the card. When there is enough data to warrant a write to the card the data is flushed to the card. This results in duplicate data, up to the internal table size, that will exist on the internal memory and the card files.

The data is written to the card in a binary format (TOB3) with a name consisting of the datalogger's Station Name (if one is assigned), the DataTable name, and a .DAT extension (stationname.tablename.dat). The file will be dated based on the time the program was compiled in the datalogger. If the TOB3 file is copied directly from the card to a PC, the binary data must be converted to a human-readable format using CardConvert software (included in LoggerNet, PC400, PC200W, and other software). However, if the data table is collected from the card using LoggerNet or data collection software it can be stored in TOA5 or other ASCII formats.

The file created on the card becomes an extension of the table memory. Any time the table data is retrieved via LoggerNet data collection or by using the TableFile instruction, the internal SRAM is searched first, then the card. If a file is found on the card with the same name, the data will be appended to that file if the file header is identical. The fields included in this header check are model type, serial number, station name, program name, and all table field information. Thus, to be identical the file must have been created on the same datalogger using the same program, with no changes to the table. If it is not identical, the datalogger will generate a compile error and no data will be stored to the card. To resume data storage to the card, delete the old files or edit the program to rename the new ones. The datalogger can be set to delete card files automatically if the header is not identical by setting the datalogger Setting DeleteCardFilesonMismatch to true (SetSetting ( DeleteCardFilesonMismatch , -1). However, this option should be set with caution to avoid deleting important data unintentionally.


## Parameters



# StopRing (Specify Data Table Type)


Specify whether the DataTable created should be a Ring Mode table (0) or a Fill and Stop table (1). In Ring Mode, once the number of records reaches the specified size, new records are stored over the oldest records. For Fill and Stop, when the table is full no more data is stored.

Type: Constant


# Size


Defines the number of records (or rows) that should be allocated for the DataTable. The number of values (or columns) in the DataTable is determined by the output processing instructions contained in the DataTable declaration. Size can be defined as a fixed number of records or as auto-allocate. To set the table size to a fixed number of records, enter that value. To set the size to autoallocate, enter a -1. If a table is set to auto-allocate, all memory that remains after creating fixed-sized tables will be allocated to this table. If multiple DataTables are declared with a -1 for size, the available memory will be divided among the tables. The datalogger attempts to allocate memory to the tables so that all tables are filled at the same time. By default, data storage memory sectors are organized as ring memory. When the ring is full, oldest data are overwritten by newest data. Using the [FillStop](fillstop.md) statement sets a program to stop writing to the data table when it is full, and no more data are stored until the table is reset.

For the CardOut instruction, enter **-1000** to set the size of the data table on the card to the size of the data table in datalogger memory.

**NOTE:** For extended-memory dataloggers, auto-allocated data tables are automatically written to the extended internal memory (which is 72 MB), unless [CardOut()](#) or [Tablefile()](tablefile.md) is used. In the case of CardOut() or Tablefile(), data from the CPU is streamed to the card in 1 KB frames and the internal extended memory is not used. Therefore, on extended-memory dataloggers, table fill times for auto-allocated tables on the CPU are greater if CardOut() or Tablefile() is not used. However, note that total final data storage for the table is greatly extended with external memory (up to 2 GB per table). In order to tell if the datalogger has extended internal memory, view the datalogger CPU Bytes Free in File Control. Dataloggers with extended internal memory show 30 MB Bytes Free for an empty CPU, compared to 1 MB Bytes Free for dataloggers that do not have extended internal memory.

Type: Constant (or expression that evaluates as a constant)

**NOTE:** If CardOut is set to Fill and Stop, when data storage stops in the card it will also stop in the data table, regardless of whether the data table in the datalogger is configured as fill and stop or has reached its maximum number of records. Similarly, if a data table is set to Fill and Stop, when data storage stops in the table it will also stop in the card.
```
