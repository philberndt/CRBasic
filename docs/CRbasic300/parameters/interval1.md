# Interval (Time Interval)

The Interval is how frequently data will be stored to the DataTable, based on the datalogger's real-time clock. Enter the time interval on which the data in the table is to be recorded. The interval may be in milliseconds, seconds, or minutes, whichever is selected with the Units parameter. Enter 0 to make the data interval the same as the scan interval. DataInterval does not override the trigger in the DataTable instruction. If the trigger is not always true, it is a condition that must be met in addition to the time interval prior to data being stored.

Type: Constant (or expression that evaluates as a constant)
