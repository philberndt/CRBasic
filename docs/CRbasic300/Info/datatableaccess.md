# Data Table Access

The datalogger has syntax that can be used to access data from a table, or to access information relating to the data in a table (Data Table or Status Table). Because of the format of this syntax, it must be entered directly into the CRBasic program (except for the instruction GetRecord, which is included in the instruction list). A brief description of the syntax that can be used follows, click the link for additional information:

- [GetRecord](../Instructions/getrecord.md): Used to store one or more records from a specified DataTable into an array.

- [TableName.EventCount](../Instructions/tablenameeventcount.md): Used to access the number of data storage events that have occurred in an event driven DataTable.

- [TableName.EventEnd](../Instructions/tablenameeventend.md): Used to determine when the last record of an event is sent to the DataTable.

- [TableName.FieldName](../Instructions/tablenamefieldname.md): Used to access data from a specific Field of a DataTable Record.

- [TableName.Output](../Instructions/tablenameoutput.md): Used to ascertain whether data was written to the Output DataTable the last time that the DataTable was called.

- [TableName.Record](../Instructions/tablenamerecord.md): Returns the record number of the record output n records back.

- TableName.TableBytes: Returns the size allocation, in number of bytes, of the selected DataTable.

- [TableName.Tablefull](../Instructions/tablenametablefull.md): Returns either 1 or 0 to indicate whether a fill and stop table is full or whether a ring-mode table has begun overwriting its oldest data.

- [TableName.TableSize](../Instructions/tablenametablesize.md): Returns the size allocation, in number of records, of the selected DataTable.

- [TableName.TimeStamp](../Instructions/tablenametimestamp.md): Returns one time stamp element of the record output n records back.
