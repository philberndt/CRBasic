# Lapses

The datalogger stores a timestamp and record number in the header of each of the table s data frames (about once per KByte of memory). Data tables using the [DataInterval](../Instructions/datainterval.md) instruction allow for a more efficient use of memory because, instead of storing time stamps and record numbers with every record, they can use the data frame s timestamp and record number information. As each new record is stored, time is checked to ensure that the time interval is correct or uniform. A lapse is any discontinuity in the records time intervals. Lapses can be the result of skipped scans, event driven tables, and/or logic in the calling of the data table from the program. For example, if the data output is controlled by a trigger (for example, a user flag) in the DataTable instruction as well as by the DataInterval instruction, lapses would occur each time the trigger was false for a period of time longer than the interval. It should be noted that if multiple data storage intervals are skipped sequentially, it is a single lapse.

As each new record is written to the data table, the datalogger checks to insure that a lapse has not occurred. If a lapse has occurred, the remaining, empty, memory of the current data frame is sacrificed, and a new data frame is started, complete with its header that includes the current timestamp and record number.

The Lapse parameter specifies the number of sub-headers for which additional memory will be allocated. The allocation is an integral number of data frames. For example, if the Lapse parameter were set to 400, the minimum memory required would be 6400 bytes (Lapse x 16 Bytes/Sub-header = 6400 bytes). If the data frames were 1 kByte, then 7 additional data frames would be allocated for the data table.

If more lapses occur than have been allocated for, new Lapse sub-headers will still be inserted into the data frames using up memory that was originally allocated for data records. The consequence of this is that the actual number of records written to the data table may be less than what was specified in the DataTable instruction.

Entering 0 for the Lapses parameter forces every record to include a record number and timestamp, requiring an additional 16 bytes per record. If data storage space is not an issue, this option should be used.

Entering a negative number for the Lapses parameter sets the datalogger to not adjust for lapses. Only the periodic data frame header time stamps (approximately once per K of data) are inserted. If a lapse occurs, a sub-header with time stamp will not be inserted, and the timestamps for subsequent records in that data frame will be generated incorrectly.

If it is known that a table is experiencing multiple lapses, it may be a more efficient use of memory to timestamp every record (Lapse parameter = 0) rather than sacrificing the remaining memory of multiple data frames due to the lapses.

Type: Constant (or expression that evaluates as a constant)
