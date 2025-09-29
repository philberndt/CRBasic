# Interval

Used only when streaming data directly from a data table or data table field. If greater than 0, the Interval parameter determines the interval at which previously unsent data will be written to file. If equal to zero, the NumRecs parameter will control when data is written. A negative Interval will cause the datalogger to write the most recent records within this time interval each time the function is called.

| NumRecs/ TimeIntoInterval | Interval | Sent                                                                                                                                                                                                                                                            |
| ------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0                         | 0        | All new (unsent) data in the data table is sent.                                                                                                                                                                                                                |
| >=0                       | >0       | Interval s worth of previously unsent records. Only previously unsent data will be sent. If multiple intervals have passed between calls to this function, multiple files will be written to the server and each file will contain interval s worth of records. |
| >0                        | 0        | Number of previously unsent records. Only previously unsent data will be sent. If multiples of NumRecs have been recorded between calls to this function, multiple files will be written to the server and each file will contain NumRecs of records.           |
| <0                        | 0        | Each time this function is called, the most recent records up to NumRecs will be sent.                                                                                                                                                                          |
| 0                         | <0       | Each time this function is called, the most recent records within this time interval will be sent.                                                                                                                                                              |

Type: Constant
