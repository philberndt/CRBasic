# Interval (Write Options)

Determines whether the instruction will write files based on a specified number of records or on a time interval. If Interval is set to 0, files will be written once a specified number of records is available in the datalogger's data table. If Interval has a non-zero value, files will be written based on a time interval (which is determined by using three parameters: TimeIntoInterval, Interval, and Units).

When using an SC115, if Interval is set to -1, all data or a specified number of records will be written when the media is plugged into the datalogger or when CardFlush is executed. All data is set by using 0 in the NumRecs parameter, and a specified number of records is set by putting the desired number of records in NumRecs.

Type: Constant or Variable
