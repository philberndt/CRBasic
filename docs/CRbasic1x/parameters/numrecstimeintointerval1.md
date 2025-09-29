# NumRecs/TimeIntoInterval (Number of Records/Time to Interval)

The function of this parameter depends upon whether or not Interval is a non-zero value. If Interval is set to 0, enter the number of records to include in each file. A new file will not be written until enough records have been written to the datalogger's table to satisfy the NumRec parameter. If Interval is a non-zero value, enter the time into the interval (or offset) that a file should be written. For instance, if Interval is set to 60, Units is set to minutes, and this parameter is set to 15, records will be written at 15 minutes past the hour, each hour.

Type: Constant or Variable
