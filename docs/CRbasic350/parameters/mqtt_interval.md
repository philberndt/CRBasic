# Interval

If Interval is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unsent records that will be published. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will cause the datalogger to send the most recent records up to NumRecs each time the function is called.

If greater than 0, the Interval parameter determines the interval at which previously unsent data will be published.

The datalogger keeps track of the last record sent in the data table information. This information is maintained even through power-downs, as long at the data table in datalogger memory is intact.

Type: Constant
