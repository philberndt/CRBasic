# MQTTPublishTable (Publish Datalogger Table to MQTT)

MQTTPublishTable is used to publish the contents of a data table using MQTT. The instruction is inserted between the DataTable and EndTable instruction. See [MQTT](https://help.campbellsci.com/CR300/Content/shared/Communication/mqtt/mqtt.htm) for more information on the use of MQTT inCampbell Scientificdata loggers.

## Syntax

MQTTPublishTable( QoS, NumRecs/TimeIntoInterval, Interval, Units, Output Format, Longitude, Latitude, Altitude)

```
'In this example program, records from the Test table are published once every 5 minutes.

'Declare Variables
Public PTemp, Batt_volt

'Define Data Tables
DataTable (Test,1,-1)'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,False,False)
Sample (1,PTemp,FP2)
MQTTPUblishTable(MQTT QoS, Number of records to publish, Interval, Interval units, Output Format, Longitude, Latitude, Altitude)
```

Note: Longitude, Latitude, and Altitude apply only if using GeoJSON format; if using GeoJSON and these values are not known, enter zeros for Longitude, Latitude and Altitude.
MQTTPublishTable(1, 0, 5, Min, 2, -111.85, 41.76, 1)'Output Format: 1 = CSIJSON; 2 = GeoJSON
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)

PanelTemp (PTemp,4000)
Battery (Batt_volt)

'Call Output Tables
CallTable Test
NextScan
EndProg

```

## Remarks


MQTTPublishTable will publish the contents of the data table inCSIJSON

**Note:**Campbell Scientific Incorporated JSON. A structured format developed by Campbell Scientific to make data from data loggers machine-readable and easy to parse in data processing environments.

orGeoJSON

**Note:**An extension of the standard GeoJSON format. It s used to represent geospatial data collected from Campbell Scientific data loggers and sensors.

format; see the Output Format parameter. This is a non-blocking instruction and will not delay CRBasic processing.


### MQTT topic structure


For data loggers with a Unique Identification Number (UID), the MQTTPublishTable instruction will publish topics using this topic structure:

> <base topic>/data/<UID>/<table name>

where

> <base topic> is determined by the datalogger**MQTTBaseTopic**setting. Its default value is cs/v1.
>
> <UID> is the datalogger UID.
>
> <table name> is the name of the table being published.

For data loggers without a UID, the MQTTPublishTable instruction will publish topics using this topic structure:

> <base topic>/data/<model>/<serial>/<table name>

where

> <base topic> is determined by the datalogger**MQTTBaseTopic**setting. Its default value is cs/v1.
>
> <model> is the datalogger model.
>
> <serial> is the datalogger serial number.
>
> <table name> is the name of the table being published.

A /cj is appended to the topic when publishing CSIJSON formatted data. For example:

> cs/v1/data/<model>/<serial>/<tableName>/cj

This allows the CLOUD ingestion tools to differentiate the format of the incoming data. Data with no /cj is GeoJSON formatted data.

**NOTE:**The maximum MQTT packet size is 4800 bytes.


## Parameters



# QoS


Publish Quality of Service. Supports levels 0 and 1 per MQTT v3.1.1. Level 2 is not supported.


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

# Interval

If Interval is equal to 0, the NumRecs/TimeIntoInterval parameters specifies the number of previously unsent records that will be published. If Interval is equal to 0, a negative NumRecs/TimeIntoInterval parameter will cause the datalogger to send the most recent records up to NumRecs each time the function is called.

If greater than 0, the Interval parameter determines the interval at which previously unsent data will be published.

The datalogger keeps track of the last record sent in the data table information. This information is maintained even through power-downs, as long at the data table in datalogger memory is intact.

Type: Constant

# Units

Specifies the units on which the TimeIntoInterval and Interval parameters will be based. The options are seconds, minutes, hours, or days.

Type: Constant

# Output Format

Specifies the format for the published data.

| Number | Description                           |
| ------ | ------------------------------------- |
| 1      | CSIJSON                               |
| 2      | Campbell ScientificVersion of GeoJSON |

**NOTE:** Note: If publishing to CampbellCloud, use output format 2, GeoJSON.

#### CSIJSON Output Format

```
{
"head" : {
"transaction" : 0,
"signature" : 49005,
"environment" : {
"station_name" : "321",
"table_name" : "panelTempC",
"model" : "CR1000X",
"serial_no" : "321",
"os_version" : "CR1000X.02.00.2019.02.27.1326",
"prog_name" : "CPU:mqttPub24.CR1X"
},
"fields" : [ {
"name" : "crTemp",
"type" : "xsd:float",
"process" : "Smp",
"settable" : false
} ]
},
"data" : [ {
"time" : "2019-02-28T18:45:00",
"vals" : [ 24.73 ]
}]
```

}

```
Type: Constant


#### GeoJSON Output Format


```

{
"type": "Feature",
"geometry": {
"type": "Point",
"coordinates": [ "12345.000000", "23456.000000", "345.000000" ]
},
"Properties": {
"loggerID": "CR1000X_400",
"observationNames": ["batteryVolt", "batteryVolt_Avg", "crTemp"],
"observations": {
"2019-02-28T19:00:00.0Z": [13.67, 13.67, 24.31]
}
}
}

```
For details, see [GeoJSON format details](mqtt_geojson_format.md).


## Optional Parameters


**NOTE:** The Longitude, Latitude, and Altitude parameters apply only to GeoJSON. If using GeoJSON and the values are unknown, enter 0. If using CSIJSON, these values are not required.


# Longitude


Only used with GEOJSON format.

A constant or variable that provides the longitude for the data logger station. 0 to -180 is west of the Greenwich meridian; 0 to 180 is east of the Greenwich meridian. Enter 0 if the value is not know.

Type: Constant or Variable


# Latitude


Only used with GEOJSON format.

A constant or variable that provides the latitude for the data logger station. The range for Latitude is 0 to 90 (positive value for Northern hemisphere and negative value for Southern hemisphere). Enter 0 if the value is not known.

Type: Constant or Variable


# Altitude


Only used with GEOJSON format.

A constant or variable that provides the altitude above sea level for the data logger station. This value must be in meters. Enter 0 if the value is not known.

Type: Constant or Variable
```
