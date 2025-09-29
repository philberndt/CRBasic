# NumRecs/TimeIntoInterval (Time into Interval to Write the Number of Records)

Used only when streaming data directly from a data table or data table field.

**NOTE:** For more details, see the [Data Streaming document](https://s.campbellsci.com/documents/us/technical-papers/ftp-streaming.pdf).

If Interval is greater than 0, the NumRecs/TimeIntoInterval parameter specifies the time into the interval at which previously unsent records should be written to file on the server.

If Interval is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unsent records that will be written to file on the server. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will specify the number of records that will be written to file on the server each time the function is called.

Type: Constant, optional parameter
