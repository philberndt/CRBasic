# NumRecs/TimeIntoInterval (Time into Interval to Publish the Number of Records)

If Interval parameter is greater than 0, the NumRecs/TimeIntoInterval parameter specifies the time into the interval at which previously unpublished records should be published

If Interval parameter is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unpublished records that will be published. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will specify the number of records that will published each time the function is called.

The datalogger keeps track of the last record sent in the data table information. This information is maintained even through power-downs, as long at the data table itself in datalogger memory is intact.

Type: Constant

Examples

These MQTTPublishTable parameters publish the most recent 2 data table records, once per minute in the GeoJSON format.

```
DataTable (panelTempC,True,-1)
DataInterval (0,1,Min,10)
Sample (1,crTemp,FP2)
MQTTPublishTable(1,2,0,Sec,2,123.00,123.000,234.000)
EndTable
```

These MQTTPublishTable parameters publish the most recent record every 30 seconds in the CSIJSON format..

```
DataTable (loggerBattery,True,-1)
DataInterval (0,30,Sec,10)
Sample(1,batteryVolt,FP2)
MQTTPublishTable(publishQoS_1,-1,0,Sec,1)
```

EndTable

```

```
